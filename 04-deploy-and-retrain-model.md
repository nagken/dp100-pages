# Domain 4 - Deploy and Retrain a Model

> **Weight: 10-15%** - Smaller domain but heavy on decisions: which endpoint, which deployment strategy, how to monitor, when to retrain. Online endpoints rotate hard on the exam.

---

## Mind map

```mermaid
mindmap
  root((04 Deploy and Retrain))
    Online endpoints
      Managed online endpoint
      Kubernetes online endpoint k8s_online
      Authentication key token aml_token
      Deployments blue green
      Traffic split
      Mirror traffic
      Autoscaling rules CPU memory custom
      Instance type DSv2 NCv3 etc
    Batch endpoints
      Batch deployment
      Compute target cluster
      mini_batch_size
      Concurrency settings
      Outputs to data store
    Monitoring
      Endpoint metrics request latency throttle
      Application Insights traces
      Data collection inputs and outputs
      Data drift monitor
      Model performance monitor
    Retraining and CI CD
      Pipeline schedules
      Triggers data new file workspace event
      Azure DevOps task v2 ML
      GitHub Actions azure ml
      MLOps maturity
```

---

## Endpoint decision tree

```mermaid
flowchart TD
    Q1{Need response<br/>in seconds?}
    Q1 -- Yes --> Q2{Bring your own<br/>k8s cluster?}
    Q2 -- Yes --> K8S[Kubernetes online endpoint<br/>type: kubernetes]
    Q2 -- No --> MOE[Managed online endpoint<br/>type: managed]
    Q1 -- No --> Q3{Score lots of records<br/>async, files in storage?}
    Q3 -- Yes --> BATCH[Batch endpoint<br/>type: batch]
    Q3 -- No --> Q1
```

| Endpoint | Latency | Throughput | Compute | Auth |
|---|---|---|---|---|
| **Managed online** | ms | medium | Microsoft-managed VMs | key, AML token, Microsoft Entra token |
| **Kubernetes online** | ms | high (you scale) | your AKS / Arc cluster | same |
| **Batch** | minutes-hours | huge | your compute cluster / serverless | key, AML token, Microsoft Entra token |

---

## Online deployment topology

```mermaid
flowchart LR
    CLIENT[Client] --> EP[Online endpoint<br/>scoring URL]
    EP --> TRAFFIC{Traffic split}
    TRAFFIC -->|90%| BLUE[Deployment: blue<br/>v1 of model]
    TRAFFIC -->|10%| GREEN[Deployment: green<br/>v2 of model]
    EP -->|mirror| GREEN
    BLUE --> M1[Replica 1..N]
    GREEN --> M2[Replica 1..N]
    M1 --> APPI[(Application Insights)]
    M2 --> APPI
    M1 --> COL[(Data collector<br/>Blob storage)]
    M2 --> COL
```

- **Endpoint** has a stable URL. **Deployments** sit under it (default names blue/green).
- Promote new version: deploy `green` at 0% -> mirror prod traffic -> flip traffic to `green` -> delete `blue`.
- **Mirror traffic** = duplicates the request to a deployment without sending the response back. Pure shadow test.

---

## Deployment swap patterns

```mermaid
flowchart LR
    PAT{Strategy} --> BG[Blue/Green<br/>switch 100% at once]
    PAT --> CAN[Canary<br/>5% then 25% then 100%]
    PAT --> MIR[Mirror<br/>0% to user, copy traffic]
    PAT --> AB[A/B<br/>fixed split for experiment]
```

| Pattern | Implement with | Rollback |
|---|---|---|
| Blue/Green | `traffic = {blue: 100}` -> `{green: 100}` | flip back |
| Canary | gradual `{blue: 95, green: 5}` -> `{50, 50}` -> `{0, 100}` | flip back |
| Mirror | `mirror_traffic = {green: 100}` | no user impact, no rollback needed |
| A/B | hold split (e.g. `{blue: 50, green: 50}`) over time | analyze metrics, then flip |

---

## Batch endpoint flow

```mermaid
flowchart LR
    INPUT[Input data asset<br/>uri_folder of CSVs] --> JOB[Batch scoring job]
    JOB --> CC[Compute cluster<br/>autoscale]
    CC --> SCORE[score.py over mini_batch_size files]
    SCORE --> OUT[Output URI<br/>predictions.csv to datastore]
```

- **`mini_batch_size`** = files per task invocation. Tune for memory.
- **`max_concurrency_per_instance`** = parallel task processes per VM.
- **`error_threshold`** = how many record-level failures to allow before failing the job.
- For MLflow models: **no `score.py`** needed. Just supply the model and input data shape.

---

## Authentication options

| Auth mode | Token lives in | Rotates? | Best for |
|---|---|---|---|
| **Key** | endpoint resource | manual rotation | quick demos |
| **AML token** | endpoint, scoped | TTL ~1h | service-to-service |
| **Microsoft Entra token** (`aad_token`) | caller's identity | per-call | production with managed identity |

> Production rule: **Microsoft Entra token + managed identity caller** > AML token > key.

---

## Monitoring landscape

```mermaid
flowchart LR
    EP[Online endpoint] --> M1[Endpoint metrics<br/>requests, latency, 5xx, throttles]
    EP --> M2[(Application Insights<br/>traces + custom logs)]
    EP --> M3[(Data collector<br/>raw inputs/outputs to Blob)]
    M3 --> DM[Data drift monitor]
    M3 --> PM[Model performance monitor<br/>compare predictions to ground truth]
    DM --> ALERT[Action group alert]
    PM --> ALERT
```

- **Endpoint metrics** are platform metrics (Azure Monitor) - no extra setup.
- **Data collector** opt-in per deployment; logs payloads to Blob with timestamps. Required to feed drift / perf monitors.
- **Data drift monitor** compares production input distribution to a baseline (training set). Triggers when a feature shifts beyond a threshold.
- **Model performance monitor** needs **ground-truth labels** joined to predictions - typically via a delayed pipeline.

---

## Retraining triggers

```mermaid
flowchart TD
    T1[Time schedule<br/>weekly/monthly] --> RUN[Pipeline endpoint job]
    T2[New data arrives<br/>Event Grid -> Logic App] --> RUN
    T3[Drift alert] --> RUN
    T4[Manual webhook<br/>CI/CD] --> RUN
    RUN --> EVAL[Evaluate vs current model]
    EVAL --> Q{Better metric?}
    Q -- Yes --> REG[Register new version]
    Q -- No --> STOP[Stop / log only]
    REG --> DEPLOY[Promote via blue/green]
```

- Schedules can be `cron` or `recurrence` style on the pipeline endpoint.
- Always **evaluate before promoting** - a retrained model can be worse on the holdout.

---

## CI/CD integration

```mermaid
flowchart LR
    REPO[Repo: code + components + ml.yml] --> PIPE{Trigger}
    PIPE --> ADO[Azure Pipelines<br/>AzureML extension]
    PIPE --> GHA[GitHub Actions<br/>azure/login + azureml/run]
    ADO --> JOB[Submit pipeline job<br/>az ml job create]
    GHA --> JOB
    JOB --> ART[Job artifacts + registered model]
    ART --> EP[Update deployment]
```

- Use **OIDC / workload identity federation** instead of stored secrets in both Azure DevOps and GitHub Actions.
- Workspace MSI on the storage account = no SAS, no keys.

---

## Common pitfalls

- **Endpoint stuck in `Updating`** - usually a `init()` exception. Check deployment logs.
- **Traffic split totals != 100** - endpoint update fails.
- **Mirror traffic > 50%** - not allowed; cap is 50%.
- **Forgot to enable data collection** - no inputs available for drift monitor.
- **Batch job OOM** - mini_batch_size too large for instance memory.
- **Drift monitor trained on tiny baseline** - flaps on noise. Use representative size.
- **Online endpoint with public access in private workspace** - set `public_network_access` on the endpoint.
- **Schedule TZ** - defaults to UTC; if you want local, set `time_zone` explicitly.
- **No autoscale rules** - fixed instance count = either over- or under-provisioned.

---

## Microsoft Learn

- [Online endpoints overview](https://learn.microsoft.com/azure/machine-learning/concept-endpoints-online)
- [Batch endpoints overview](https://learn.microsoft.com/azure/machine-learning/concept-endpoints-batch)
- [Safe rollout for online endpoints](https://learn.microsoft.com/azure/machine-learning/how-to-safely-rollout-online-endpoints)
- [Collect production data from models](https://learn.microsoft.com/azure/machine-learning/how-to-collect-production-data)
- [Model monitoring](https://learn.microsoft.com/azure/machine-learning/how-to-monitor-model-performance)
- [Schedule machine learning pipeline jobs](https://learn.microsoft.com/azure/machine-learning/how-to-schedule-pipeline-job)
- [MLOps with Azure ML](https://learn.microsoft.com/azure/machine-learning/concept-model-management-and-deployment)

---

[<- Prepare Model for Deployment](03-prepare-model-for-deployment.md) - [Master Index](00-MASTER-INDEX.md)
