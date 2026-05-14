# Microsoft Reference Library

> Curated entry points to the official Microsoft documentation, training, and architecture resources that complement this study guide. These links remain authoritative across exam updates and are the recommended starting points for deeper investigation.

## Exam and Skills

| Resource | Use it for |
| --- | --- |
| [DP-100 certification page](https://learn.microsoft.com/credentials/certifications/azure-data-scientist/) | Official exam overview, scheduling, and prerequisites. |
| [DP-100 study guide (skills measured)](https://learn.microsoft.com/credentials/certifications/resources/study-guides/dp-100) | Authoritative skills outline - the primary reference for what the exam can ask. |
| [DP-100 practice assessment](https://learn.microsoft.com/credentials/certifications/azure-data-scientist/) | Free Microsoft practice questions aligned to the skills outline. |
| [Build and operate ML solutions with Azure ML (DP-100)](https://learn.microsoft.com/training/courses/dp-100t01) | Free Microsoft training aligned to DP-100. |

## Microsoft Learn - Foundational Paths

| Resource | Use it for |
| --- | --- |
| [Build and operate machine learning solutions](https://learn.microsoft.com/training/paths/build-ai-solutions-with-azure-ml-service/) | End-to-end Azure ML lifecycle. |
| [Train and manage a machine learning model](https://learn.microsoft.com/training/paths/train-and-manage-machine-learning-model-with-azure-machine-learning/) | Compute, data, jobs, sweeps, pipelines. |
| [Deploy and consume models with Azure ML](https://learn.microsoft.com/training/paths/deploy-consume-models-azure-machine-learning/) | Online and batch endpoints, safe rollout, monitoring. |
| [DP-100 hands-on labs (mslearn-azureml)](https://microsoftlearning.github.io/mslearn-azureml/) | Self-paced exercises mapped to skills measured. |

## Workspace and Compute

| Resource | Use it for |
| --- | --- |
| [What is Azure Machine Learning?](https://learn.microsoft.com/azure/machine-learning/overview-what-is-azure-machine-learning) | Top-level concept introduction. |
| [SDK / CLI v2 overview](https://learn.microsoft.com/azure/machine-learning/concept-v2) | What changed from v1 and which APIs to use. |
| [Compute instance](https://learn.microsoft.com/azure/machine-learning/concept-compute-instance) | Single-user authoring VM. |
| [Compute cluster](https://learn.microsoft.com/azure/machine-learning/how-to-create-attach-compute-cluster) | Autoscaling cluster for jobs. |
| [Serverless compute](https://learn.microsoft.com/azure/machine-learning/how-to-use-serverless-compute) | Per-job compute, no cluster management. |
| [Kubernetes compute](https://learn.microsoft.com/azure/machine-learning/how-to-attach-kubernetes-anywhere) | Bring-your-own-cluster (AKS / Arc-enabled). |

## Data and Environments

| Resource | Use it for |
| --- | --- |
| [Datastores](https://learn.microsoft.com/azure/machine-learning/how-to-datastore) | Identity-based vs credential-based connections. |
| [Data assets](https://learn.microsoft.com/azure/machine-learning/how-to-create-data-assets) | `uri_file`, `uri_folder`, and MLTable assets. |
| [Identity-based data access](https://learn.microsoft.com/azure/machine-learning/how-to-identity-based-data-access) | Workspace MSI patterns and required RBAC. |
| [Environments overview](https://learn.microsoft.com/azure/machine-learning/concept-environments) | Curated and custom Conda + Docker definitions. |

## Training and Pipelines

| Resource | Use it for |
| --- | --- |
| [Train models with the SDK](https://learn.microsoft.com/azure/machine-learning/how-to-train-model) | Command jobs end-to-end. |
| [Hyperparameter tuning (sweep)](https://learn.microsoft.com/azure/machine-learning/how-to-tune-hyperparameters) | Sampling strategies and termination policies. |
| [Build pipelines with components](https://learn.microsoft.com/azure/machine-learning/how-to-create-component-pipeline-python) | Reusable steps and DAG authoring. |
| [Parallel job in a pipeline](https://learn.microsoft.com/azure/machine-learning/how-to-use-parallel-job-in-pipeline) | Map a script over input partitions. |

## AutoML

| Resource | Use it for |
| --- | --- |
| [AutoML overview](https://learn.microsoft.com/azure/machine-learning/concept-automated-ml) | When and why to use AutoML. |
| [Tabular AutoML](https://learn.microsoft.com/azure/machine-learning/how-to-configure-auto-train) | Classification and regression on MLTable. |
| [AutoML for vision](https://learn.microsoft.com/azure/machine-learning/how-to-auto-train-image-models) | Image tasks. |
| [AutoML for NLP](https://learn.microsoft.com/azure/machine-learning/how-to-auto-train-nlp-models) | Text classification and NER. |
| [AutoML for forecasting](https://learn.microsoft.com/azure/machine-learning/how-to-auto-train-forecast) | Time-series with `time_column_name`. |

## MLflow and Models

| Resource | Use it for |
| --- | --- |
| [MLflow tracking](https://learn.microsoft.com/azure/machine-learning/how-to-use-mlflow-cli-runs) | Logging params, metrics, artifacts. |
| [Manage models with MLflow](https://learn.microsoft.com/azure/machine-learning/how-to-manage-models-mlflow) | Registry and stages. |
| [Deploy MLflow models](https://learn.microsoft.com/azure/machine-learning/how-to-deploy-mlflow-models-online-endpoints) | No-code deploy to online endpoints. |

## Deployment and Monitoring

| Resource | Use it for |
| --- | --- |
| [Online endpoints](https://learn.microsoft.com/azure/machine-learning/how-to-deploy-online-endpoints) | Real-time scoring. |
| [Batch endpoints](https://learn.microsoft.com/azure/machine-learning/how-to-use-batch-endpoint) | Async file/folder scoring. |
| [Safe rollout](https://learn.microsoft.com/azure/machine-learning/how-to-safely-rollout-online-endpoints) | Blue/green and mirror traffic. |
| [Autoscale endpoints](https://learn.microsoft.com/azure/machine-learning/how-to-autoscale-endpoints) | CPU and schedule rules. |
| [Monitor online endpoints](https://learn.microsoft.com/azure/machine-learning/how-to-monitor-online-endpoints) | Built-in metrics and Log Analytics. |
| [Data collector](https://learn.microsoft.com/azure/machine-learning/how-to-collect-production-data) | Required for drift monitoring. |
| [Model monitoring](https://learn.microsoft.com/azure/machine-learning/how-to-monitor-model-performance) | Drift, performance, data quality. |

## Responsible AI

| Resource | Use it for |
| --- | --- |
| [Responsible AI overview](https://learn.microsoft.com/azure/machine-learning/concept-responsible-ai) | Six principles and Azure ML support. |
| [RAI dashboard concept](https://learn.microsoft.com/azure/machine-learning/concept-responsible-ai-dashboard) | Error analysis, fairness, SHAP, counterfactual, causal. |
| [Build the RAI dashboard](https://learn.microsoft.com/azure/machine-learning/how-to-responsible-ai-dashboard) | Pipeline component composition. |

## Security and Network Isolation

| Resource | Use it for |
| --- | --- |
| [Network isolation planning](https://learn.microsoft.com/azure/machine-learning/how-to-network-isolation-planning) | VNet vs managed VNet. |
| [Managed VNet](https://learn.microsoft.com/azure/machine-learning/how-to-managed-network) | Microsoft-managed isolation. |
| [Workspace RBAC](https://learn.microsoft.com/azure/machine-learning/how-to-assign-roles) | Built-in roles and scoping. |
| [Managed identities](https://learn.microsoft.com/azure/machine-learning/how-to-use-managed-identities) | System and user-assigned MSI for jobs and endpoints. |

## CLI and SDK References

| Resource | Use it for |
| --- | --- |
| [Azure ML CLI v2](https://learn.microsoft.com/cli/azure/ml) | Command reference. |
| [Python SDK v2](https://learn.microsoft.com/python/api/overview/azure/ai-ml-readme) | `MLClient` and entity reference. |
| [REST API](https://learn.microsoft.com/rest/api/azureml/) | Direct REST integration. |

## Architecture Center

| Resource | Use it for |
| --- | --- |
| [MLOps with Azure Machine Learning](https://learn.microsoft.com/azure/architecture/example-scenario/mlops/mlops-technical-paper) | End-to-end MLOps reference. |
| [Real-time scoring](https://learn.microsoft.com/azure/architecture/reference-architectures/ai/real-time-scoring-machine-learning-models) | Online endpoint architecture. |
| [Batch scoring of deep learning models](https://learn.microsoft.com/azure/architecture/reference-architectures/ai/batch-scoring-deep-learning) | Batch endpoint architecture. |

[<- Master Index](00-MASTER-INDEX.md)
