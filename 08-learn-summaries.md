# Microsoft Learn Summaries

> Tight, exam-focused summaries of every major Azure Machine Learning concept covered in this guide. Each entry pairs a one-paragraph **what it is** with the **exam relevance**.

> Use this page when you want a 60-second refresher on a concept before diving into the domain pages.

---

## Azure Machine Learning Workspace

The top-level resource that ties together compute, datastores, models, environments, jobs, and endpoints. Backed by linked Azure Storage, Key Vault, ACR, and Application Insights. **Exam:** know the linked resources by heart, know that workspace MSI is what jobs use to access data when configured for identity-based access.

## Compute Instance

Single-user VM optimized for authoring (notebooks, VS Code, RStudio). Always-on; **does not scale to 0**. Has built-in Jupyter / VS Code Server / Terminal. Idle shutdown is opt-in. **Exam:** the answer when the question says "data scientist authoring code".

## Compute Cluster (AmlCompute)

Multi-node autoscaling cluster (`min_instances`..`max_instances`). Used for training, sweep, pipeline, parallel jobs. Scales to 0 when idle. **Exam:** answer when the question says "train at scale", "hyperparameter tuning", or "pipeline".

## Serverless compute

Pay-per-job, no cluster management. The runtime picks the SKU you specify and disposes after. **Exam:** answer when "no infrastructure to manage" or "no quota planning" appears.

## Datastore

A pointer to an Azure Storage location plus an authentication mode. Built-in `workspaceblobstore` is created at workspace creation. Modes: **credential-based** (account key/SAS) or **identity-based** (workspace MSI / user identity). **Exam:** "no stored credentials" -> identity-based.

## Data Asset

A versioned, named reference to data. Three types: `uri_file`, `uri_folder`, `mltable`. **Exam:** AutoML tabular = `mltable`. Single CSV for a command job = `uri_file`. Folder of training images = `uri_folder`.

## Environment

Reproducible Python runtime: curated, custom (conda + Docker), or fully Docker-context. **Exam:** when the framework version isn't curated, build a custom env. Inference custom envs need `azureml-inference-server-http`.

## Command Job

`type: command` - runs one script with one set of inputs. Foundational job type. **Exam:** baseline answer for "train a model with these args".

## Sweep Job

`type: sweep` - wraps a command job and adds search space + sampler + termination + concurrency. **Exam:** "tune hyperparameters" -> sweep job. Match sampling strategy and termination policy correctly.

## Pipeline Job

`type: pipeline` - DAG of components. Inputs/outputs flow between steps. **Exam:** "multi-step workflow" or "reusable training step" -> pipeline + components.

## Parallel Job

`type: parallel` - partitions input data and runs the same script in parallel. **Exam:** "score 10M files concurrently" or "feature-engineer per partition".

## MLflow Tracking

Open-source experiment tracking baked into Azure ML. `mlflow.autolog()` captures sklearn/pytorch/etc. metrics + params + model. `MLClient` + workspace tracking URI auto-set inside jobs. **Exam:** know `autolog` and `log_model` flavors; know that a logged MLflow model = no-code deployment.

## AutoML

Automatic model search across algorithms + featurization + HP tuning for classification, regression, forecasting, vision, NLP. **Exam:** know primary metric per task, featurization modes (`auto`/`off`/`custom`), and that forecasting needs `time_column_name`.

## Hyperparameter Sampling

Grid (exhaustive on `choice()`), Random (any distribution), Bayesian (continuous only, sequential, **no early termination**). **Exam:** match sampling to constraint.

## Early Termination

Bandit (slack-based), Median Stopping (vs running median), Truncation Selection (drop bottom %). Saves cost. **Exam:** Bayesian sampling = no policy.

## Distributed Training

`distribution.type` = `pytorch` (DDP), `tensorflow` (parameter server / multi-worker), `mpi` (Horovod). Cluster needs InfiniBand SKU for efficient multi-GPU. **Exam:** know `process_count_per_instance`.

## Responsible AI Dashboard

Composed of components: error analysis, fairness, interpretability (SHAP/mimic), counterfactual, causal. Built via pipeline; renders on registered model. **Exam:** "explain a single prediction" -> counterfactual or local SHAP.

## Model Registry

Versioned models in the workspace (`mlflow_model`, `custom_model`, `triton_model`). **Exam:** types matter - only `mlflow_model` deploys without a scoring script.

## Online Endpoint

Real-time HTTP endpoint with one or more deployments. Variants: **managed** (Microsoft VMs) and **kubernetes** (your AKS/Arc). **Exam:** ms-latency response -> online; "use my AKS" -> kubernetes online.

## Batch Endpoint

Async scoring over data assets, output to a datastore. **Exam:** "score millions of rows" -> batch.

## Deployment Strategies

Blue/green (instant cutover), canary (gradual %), mirror (shadow, no user response), A/B (steady split). **Exam:** "shadow new version" -> mirror traffic.

## Data Collector

Logs raw inputs/outputs from an online deployment to Blob. **Exam:** prerequisite for drift / perf monitors.

## Data Drift Monitor

Detects distribution shift in features vs a baseline. **Exam:** "inputs no longer match training" -> drift monitor.

## Model Performance Monitor

Compares predictions to delivered ground-truth labels. **Exam:** "model accuracy degraded over time" -> perf monitor.

## Pipeline Endpoint

Published, versioned, schedulable pipeline. Triggered by schedule, REST call, or event. **Exam:** retraining pattern.

## Workload Identity Federation (OIDC)

Federated trust from GitHub / Azure DevOps to Microsoft Entra - no stored secrets in CI/CD. **Exam:** "no stored credentials in CI/CD" -> OIDC + service connection / federated credential.

---

[<- Master Index](00-MASTER-INDEX.md)
