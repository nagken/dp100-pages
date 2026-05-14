# Common Pitfalls and Distractor Patterns

> Mistakes that look right on the DP-100 exam but lose points. Each entry pairs the wrong choice candidates pick with the correct one and the rule.

## Compute and Workspace

### Compute instance left running 24/7

**Pitfall**: "Spin up a compute instance for the data scientist."

**Reality**: Compute instance **does not scale to 0**. Without idle-shutdown configured, it bills around the clock. Always set idle-shutdown or stop manually. Defaults differ from compute clusters.

### Compute cluster with `min_instances > 0`

**Pitfall**: Setting `min_instances: 1` "to keep the cluster warm."

**Reality**: Same billing trap as compute instance. Default to `min_instances: 0`; only raise it when warm capacity is required for SLA.

### GPU SKU not available in your region

**Pitfall**: Picking NDv2 or ND_A100 in any region.

**Reality**: GPU quota is region-scoped and scarce. Validate with `az vm list-skus` and quota requests **before** workspace create.

### Workspace and storage in different regions

**Pitfall**: Reusing an existing storage account regardless of region.

**Reality**: Egress + cross-region latency on every job. Co-locate workspace, default storage, ACR, and Key Vault in the same region.

## Data

### AutoML tabular fed `uri_folder`

**Pitfall**: Pointing AutoML at a folder of CSVs.

**Reality**: AutoML tabular requires **MLTable** with declared schema. `uri_folder` and `uri_file` are not valid inputs for AutoML.

### Forecasting without `time_column_name`

**Pitfall**: Treating the timestamp like any other feature.

**Reality**: AutoML forecasting requires `time_column_name`. Missing it triggers an error at submit time.

### Identity-based datastore but no storage RBAC

**Pitfall**: Switching to identity-based, expecting it to "just work."

**Reality**: Workspace MSI needs **Storage Blob Data Contributor** (or Reader for read-only) on the storage account. Identity-based without RBAC fails on first read.

### Credential-based datastore in production

**Pitfall**: Using account key or SAS for the production datastore.

**Reality**: Secrets land in workspace Key Vault and require manual rotation. Prefer **identity-based** with workspace MSI + RBAC.

## Training and Sweeps

### Bayesian sampling with early termination

**Pitfall**: Pairing Bayesian sampling with a Bandit policy.

**Reality**: **Invalid combination**. Bayesian must run trials to completion to update its model. Use Random or Grid sampling if you want early termination.

### Grid sampling with continuous distributions

**Pitfall**: Using `uniform()` or `normal()` with `grid` sampling.

**Reality**: Grid only supports `choice()`. Continuous distributions explode the search. Switch to Random or Bayesian for continuous spaces.

### Forgot `goal: maximize` on the primary metric

**Pitfall**: Optimizing accuracy without setting `goal`.

**Reality**: Default is **minimize**, so the sweep picks the lowest-accuracy run as best. Always declare `goal: maximize` for accuracy/AUC/F1.

### Distributed PyTorch with `process_count_per_instance: 1`

**Pitfall**: Single-process per node despite multiple GPUs.

**Reality**: Only one process runs; gradients aren't synced across GPUs. Set `process_count_per_instance` equal to GPUs per node.

### `max_concurrent_trials > cluster max_instances`

**Pitfall**: Cranking concurrency higher than the cluster size.

**Reality**: Sweep slows to a crawl as runs queue. Match concurrency to cluster `max_instances` (or lower).

### MLflow autolog logs nothing

**Pitfall**: Calling `mlflow.autolog()` and assuming everything is captured.

**Reality**: Framework version must be in the supported list. Pin compatible versions or fall back to manual `mlflow.log_*`.

## Pipelines and Components

### Pipeline cache reuses a step that depends on today's data

**Pitfall**: Letting AML reuse a daily ETL step.

**Reality**: AML caches deterministic steps by inputs + code hash. Set `is_deterministic: false` for steps whose output changes with wall-clock time.

### Component code path too large

**Pitfall**: Submitting a component from the repo root.

**Reality**: Slow uploads + long iteration time. Add `.amlignore` for unrelated files (notebooks, data, build outputs).

## Models and Deployment

### Logged an artifact instead of an MLflow model

**Pitfall**: `mlflow.log_artifact("model.pkl")`.

**Reality**: That's an artifact, not a model. **No no-code deploy**. Use `mlflow.<framework>.log_model(...)` so the model is registered with signature + flavor.

### `init()` exception on online endpoint

**Pitfall**: First learning about the bug from the deployed endpoint.

**Reality**: Endpoint goes Unhealthy and traffic update fails. Always test locally with `--local` first.

### Custom inference env missing `azureml-inference-server-http`

**Pitfall**: Custom Docker env without the inference server package.

**Reality**: HTTP server never starts; deployment fails. Either base on the curated inference image or pin `azureml-inference-server-http`.

### Hard-coded `AZUREML_MODEL_DIR` path

**Pitfall**: `model = load("/var/azureml-app/models/model.pkl")` in `score.py`.

**Reality**: Works locally, breaks in the endpoint container. Always read `os.environ["AZUREML_MODEL_DIR"]`.

### Triton model with wrong directory layout

**Pitfall**: Flat `model.pt` at the root.

**Reality**: Triton requires `<model_repo>/<name>/<version>/<artifacts>` layout. Wrong shape => deployment fails.

### Online endpoint traffic split doesn't sum to 100

**Pitfall**: `blue: 50, green: 30`.

**Reality**: Update is rejected. Splits must sum to 100 across all deployments behind the endpoint.

### Mirror traffic above 50%

**Pitfall**: Mirroring 80% of traffic to a shadow deployment.

**Reality**: **Cap is 50%.** Anything higher is rejected.

### Online endpoint chosen for batch scoring

**Pitfall**: Posting a 1 GB CSV to an online endpoint.

**Reality**: Online endpoints are real-time, low-latency, single-record. Use a **batch endpoint** for files/folders.

## Monitoring and Retraining

### Drift monitor without data collection enabled

**Pitfall**: Creating a drift monitor on a deployment that never logs inputs.

**Reality**: No inputs land in storage; the monitor never triggers. Enable `data_collector` on the deployment first.

### Model performance monitor without ground-truth labels

**Pitfall**: Trying to monitor accuracy when labels never arrive.

**Reality**: Performance monitors require **joined ground truth**. Either add a labeling pipeline or stick to data-quality and data-drift monitors.

### Schedule defaults to UTC

**Pitfall**: Cron expression that "should run at 9 AM."

**Reality**: Schedule defaults to UTC. Set `time_zone` explicitly if you want local time.

### Online endpoint with no autoscale rules

**Pitfall**: Fixed instance count on a real workload.

**Reality**: Either over- or under-provisioned. Add CPU/memory or schedule-based autoscale rules to the deployment.

## Security and CI/CD

### GitHub Actions secret instead of OIDC

**Pitfall**: Storing a service principal secret in GitHub.

**Reality**: Rotation pain + leak risk. Use **OIDC federated credential** on a user-assigned managed identity. No secret in CI.

### Public workspace with private compute

**Pitfall**: Locking compute to private network but leaving the workspace public.

**Reality**: Studio + control plane still reachable from the internet. Set `public_network_access_enabled = false` for full isolation.

### Owner role on data scientists

**Pitfall**: Granting Owner "to make things work."

**Reality**: WAF security + governance fail. Use **AzureML Data Scientist** plus the right storage RBAC role on data containers.

## Looks Like Option A, Is Actually B

| Tempting wrong answer | Right answer | Why |
| --- | --- | --- |
| Compute instance for sweep | Compute cluster | Instance is single-VM, no parallelism |
| Compute cluster for notebook authoring | Compute instance | Cluster is for submitted jobs, not interactive sessions |
| `uri_folder` for AutoML tabular | MLTable | AutoML expects schema |
| Grid sampling for a big space | Random sampling | Grid explodes |
| Bayesian + Bandit | Bayesian alone, or Random + Bandit | Bayesian needs every result |
| Custom model + MLflow no-code deploy | Custom model + `score.py` | No-code only for `mlflow_model` |
| Online endpoint for batch scoring | Batch endpoint | Online is real-time, not bulk |
| Blue/Green for shadow test | Mirror traffic | Mirror = no user impact |
| Account key for production datastore | Workspace MSI + RBAC | Identity-based |

[<- Master Index](00-MASTER-INDEX.md)
