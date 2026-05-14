# Glossary and Acronym Reference

> Authoritative definitions for the Azure Machine Learning terms, acronyms, and product names that appear in DP-100 scenarios.

## Workspace and Compute

| Term | Definition |
| --- | --- |
| **AML** | Azure Machine Learning. |
| **Workspace** | Top-level Azure ML resource that ties compute, datastores, models, environments, jobs, and endpoints. |
| **Compute instance** | Single-user authoring VM. Always-on, **does not scale to 0** unless idle-shutdown is set. |
| **Compute cluster (AmlCompute)** | Autoscaling multi-node VM cluster for submitted jobs. |
| **Serverless compute** | Per-job compute managed by Azure; no cluster to provision. |
| **K8s online endpoint** | Online endpoint backed by your own AKS / Arc-enabled Kubernetes cluster. |
| **Idle shutdown** | Auto-stop policy on a compute instance after a period of inactivity. |
| **Spot VM / low-priority** | Discounted interruptible compute. Suitable for fault-tolerant batch jobs. |
| **VNet injection** | Compute deployed inside a customer VNet subnet for network isolation. |

## Data and Environments

| Term | Definition |
| --- | --- |
| **Datastore** | Pointer plus authentication metadata for an Azure Storage location. |
| **Identity-based access** | Datastore auth via workspace MSI or user identity. |
| **Credential-based access** | Datastore auth via account key or SAS stored in workspace Key Vault. |
| **Data asset** | Versioned, named reference to data of type `uri_file`, `uri_folder`, or `mltable`. |
| **MLTable** | Schema + transform spec for tabular data assets. Required input for AutoML tabular. |
| **Environment** | Conda + Docker image definition for reproducible runs. |
| **Curated environment** | Microsoft-maintained, signed Python environment (`AzureML-...` prefix). |
| **Key Vault** | Secrets/keys/certs store linked to the workspace. |
| **MSI** | Managed identity (system-assigned or user-assigned) used by workspace and jobs. |

## Jobs and Pipelines

| Term | Definition |
| --- | --- |
| **Command job** | Single-script job (`type: command`). |
| **Sweep job** | Hyperparameter tuning job (`type: sweep`). |
| **Pipeline job** | DAG of components, one-off run. |
| **Pipeline endpoint** | Published, schedulable, REST-callable pipeline. |
| **Component** | Reusable parameterized job step in a pipeline. |
| **Parallel job** | Map a script over partitions of input data. |
| **Schedule** | Cron- or recurrence-based trigger for a job or pipeline. Defaults to UTC. |

## Hyperparameter Tuning

| Term | Definition |
| --- | --- |
| **AutoML** | Automated ML - auto-search algorithms, featurization, and hyperparameters. |
| **Hyperdrive** | Older name for sweep; the tuning subsystem. |
| **Random sampling** | Fastest general-purpose HP sampling default. |
| **Grid sampling** | Exhaustive HP search over discrete `choice()` distributions only. |
| **Bayesian sampling** | Sequential, model-guided HP sampling. **No early termination** allowed. |
| **Bandit policy** | Aggressive early-termination policy - cull runs outside slack of best. |
| **Median stopping** | Conservative early termination - kill runs below running median. |
| **Truncation selection** | Early termination - drop bottom percentile each interval. |
| **Primary metric** | Metric the sweep or AutoML optimizes. Pair with `goal: maximize` or `minimize`. |

## Distributed Training

| Term | Definition |
| --- | --- |
| **DDP** | Distributed Data Parallel (PyTorch). |
| **Horovod** | Open-source distributed training (MPI-based). |
| **MPI** | Message Passing Interface - used by Horovod. |
| **NCCL** | NVIDIA collective comms - backend for distributed GPU training. |

## MLflow and Models

| Term | Definition |
| --- | --- |
| **MLflow** | Open-source tracking and registry, integrated into Azure ML. |
| **MLflow autolog** | Automatic capture of params, metrics, and artifacts for supported frameworks. |
| **MLflow model** | Model logged with signature + conda env. Enables **no-code deploy**. |
| **No-code deployment** | Deploy an MLflow model without writing a `score.py`. |
| **Model registry** | Versioned model store inside the workspace. |
| **Score script** | `score.py` with `init()` and `run()` for custom (non-MLflow) models. |
| **Triton** | NVIDIA inference server with multi-framework support. |

## Endpoints and Deployment

| Term | Definition |
| --- | --- |
| **Endpoint** | URL-stable wrapper around one or more deployments. |
| **Managed online endpoint** | Microsoft-managed VMs serving real-time inference. |
| **Batch endpoint** | Async, file/folder-based scoring endpoint. |
| **Blue/Green** | Two deployments behind one endpoint; swap traffic 0 -> 100. |
| **Canary** | Gradual traffic shift 5% -> 25% -> 100%. |
| **Mirror traffic** | Shadow-copy requests to a deployment; responses are dropped. **Capped at 50%.** |
| **AZUREML_MODEL_DIR** | Env var pointing to the deployed model directory inside the inference container. |

## Monitoring and Responsible AI

| Term | Definition |
| --- | --- |
| **Data collector** | Logs raw endpoint inputs/outputs to Blob for monitoring. |
| **Data drift monitor** | Detects feature distribution shift vs a baseline. |
| **Model performance monitor** | Compares predictions to joined ground truth. |
| **RAI dashboard** | Responsible AI dashboard (error analysis, fairness, SHAP, counterfactual, causal). |
| **SHAP** | Shapley-value-based feature attribution. |
| **Interpretability** | SHAP / mimic / LIME - explain individual predictions. |
| **Error analysis** | RAI component identifying cohorts with high error rate. |
| **Fairness** | RAI component measuring metric parity across sensitive features. |
| **Counterfactual** | RAI tool - "what change flips this prediction?". |
| **Drift** | Distribution change in features or predictions over time. |

## Security and CI/CD

| Term | Definition |
| --- | --- |
| **RBAC** | Role-based access control. |
| **AzureML Data Scientist** | Built-in role granting full workspace experimentation rights without subscription Owner. |
| **OIDC** | OpenID Connect - federated identity for CI/CD without stored secrets. |
| **Workload identity federation** | OIDC-based Azure auth from CI/CD agents. |
| **Federated credential** | Microsoft Entra OIDC trust on a managed identity, no stored secret. |
| **MLClient** | Python SDK v2 entry point (`azure.ai.ml.MLClient`). |
| **MLOps** | DevOps for ML - CI/CD plus monitoring and retraining loops. |

[<- Master Index](00-MASTER-INDEX.md)
