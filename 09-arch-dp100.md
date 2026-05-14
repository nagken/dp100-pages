# Architectures - DP-100

> Reference architectures the exam asks about, plus annotated diagrams.

---

## 1. End-to-end MLOps with Azure ML

```mermaid
flowchart LR
    DEV[Developer] -->|git push| REPO[GitHub or Azure Repos]
    REPO --> CI{CI / CD}
    CI -->|GitHub Actions or Azure Pipelines| WS[(Azure ML Workspace)]
    WS --> CC[Compute cluster]
    CC --> JOB[Pipeline job<br/>data prep + train + register]
    JOB --> MR[Model Registry]
    MR --> TEST[Test online endpoint]
    TEST --> PROD[Prod online endpoint<br/>blue/green]
    PROD --> APPI[(Application Insights)]
    PROD --> DC[(Data Collector)]
    DC --> MON[Drift + Perf monitor]
    MON -->|alert| CI
    CI -->|federated credential OIDC| AAD[Microsoft Entra ID]
```

---

## 2. Real-time inference (online managed endpoint)

```mermaid
flowchart LR
    CLIENT[Client app] -->|HTTPS scoring URL| EP[Managed online endpoint]
    EP --> BLUE[Deployment: blue<br/>v1 model]
    EP --> GREEN[Deployment: green<br/>v2 model]
    BLUE --> APPI[(App Insights)]
    GREEN --> APPI
    EP --> KV[(Key Vault<br/>endpoint MSI fetches secrets)]
    EP --> ACR[(ACR<br/>scoring image)]
```

---

## 3. Batch scoring

```mermaid
flowchart LR
    SRC[(ADLS Gen2<br/>millions of files)] --> DA[Data asset uri_folder]
    DA --> BE[Batch endpoint]
    BE --> CC[Compute cluster<br/>autoscale 0..50]
    CC --> SCORE[Parallel scoring<br/>mini_batch_size]
    SCORE --> OUT[(Predictions to ADLS)]
    OUT --> DOWNSTREAM[Power BI / Synapse]
```

---

## 4. Distributed GPU training

```mermaid
flowchart LR
    DATA[(ADLS Gen2)] --> JOB[Command job<br/>distribution.type pytorch]
    JOB --> CC[Compute cluster<br/>4 x ND_A100_v4 InfiniBand]
    CC --> N1[Node 1<br/>process_count_per_instance 8]
    CC --> N2[Node 2]
    CC --> N3[Node 3]
    CC --> N4[Node 4]
    N1 -.->|NCCL allreduce| N2
    N2 -.-> N3
    N3 -.-> N4
    N4 --> CHECK[(Checkpoints to Blob)]
    JOB --> MLF[(MLflow run)]
```

---

## 5. Private workspace (network-isolated)

```mermaid
flowchart LR
    subgraph VNET[Customer VNet]
      subgraph SUB1[Workspace subnet]
        PE[Private endpoint to workspace]
      end
      subgraph SUB2[Compute subnet]
        CC[VNet-injected compute cluster]
      end
      subgraph SUB3[Data subnet]
        ST[Private endpoint to Storage]
        KV[Private endpoint to Key Vault]
        ACR[Private endpoint to ACR]
        AI[Private endpoint to App Insights]
      end
    end
    USR[Bastion or VPN user] --> PE
    PE --> WS[(Azure ML Workspace)]
    WS --> CC
    CC --> ST
    CC --> KV
    CC --> ACR
```

> Set `public_network_access_enabled = false` on workspace; restrict storage to selected networks; ACR with private endpoint + dedicated SKU.

---

## 6. Closed-loop retraining

```mermaid
flowchart LR
    PROD[Online endpoint] --> DC[Data Collector]
    DC --> BLOB[(Production payloads)]
    LABEL[Ground-truth ingestion] --> JOIN[Joined dataset]
    BLOB --> JOIN
    JOIN --> MON[Performance monitor]
    MON -->|drift or accuracy alert| TRIG[Pipeline trigger]
    TRIG --> RETRAIN[Pipeline endpoint<br/>retrain + register]
    RETRAIN --> EVAL{Better than current?}
    EVAL -- Yes --> DEPLOY[Deploy as green]
    DEPLOY --> MIRROR[Mirror traffic]
    MIRROR --> PROMOTE[Promote to 100%]
    EVAL -- No --> NOTIFY[Notify owner]
```

---

## 7. AutoML for tabular data

```mermaid
flowchart LR
    SRC[(Training table)] --> MLT[MLTable data asset]
    MLT --> AML[AutoML job<br/>task: classification]
    AML --> CHILDREN[Many child runs<br/>algos x featurization]
    CHILDREN --> BEST[Best run by primary metric]
    BEST --> MLF[MLflow model logged]
    MLF --> EP[Managed online endpoint<br/>no-code deploy]
```

---

[<- Master Index](00-MASTER-INDEX.md)
