# Azure Architecture Center

> The **[Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)** is the single most important external resource for DP-100. It hosts every reference architecture, design pattern, and Well-Architected workload review you may be tested on. Browse the full catalog at [learn.microsoft.com/azure/architecture/browse](https://learn.microsoft.com/azure/architecture/browse/) and filter by product, category, or scenario.

## How to use it for DP-100

1. Read the **WAF for AI workloads** entry first; understand reliability, security, cost, ops, and performance trade-offs for ML systems.
2. Pick **two or three reference architectures per topic area** (real-time scoring, batch scoring, MLOps, distributed training) and walk them end-to-end.
3. Memorize the **decision points** - endpoint type, environment isolation, scoring SLA, retraining trigger.
4. Use the **browse filters** (cost, security, reliability) to find the WAF-aligned variant of each scenario.

## Top entry points

| Resource | Why it matters for DP-100 |
| --- | --- |
| [Architecture Center home](https://learn.microsoft.com/azure/architecture/) | Curated landing page; start here. |
| [Browse architectures](https://learn.microsoft.com/azure/architecture/browse/) | Filterable catalog of every reference architecture. |
| [AI and machine learning architectures](https://learn.microsoft.com/azure/architecture/ai-ml/) | Top-level AI/ML scenario index. |
| [WAF for AI workloads](https://learn.microsoft.com/azure/well-architected/ai/get-started) | Five-pillar guidance applied to AI/ML. |
| [Cloud Adoption Framework - AI](https://learn.microsoft.com/azure/cloud-adoption-framework/innovate/ai/) | Adoption, governance, and landing-zone guidance. |
| [Cloud design patterns](https://learn.microsoft.com/azure/architecture/patterns/) | Retry, throttling, queue-based load levelling - useful for inference design. |

## Reference architectures by DP-100 topic

### Real-time scoring

- [Real-time scoring of machine learning models](https://learn.microsoft.com/azure/architecture/reference-architectures/ai/real-time-scoring-machine-learning-models)
- [Real-time scoring of Python and PySpark models](https://learn.microsoft.com/azure/architecture/reference-architectures/ai/realtime-scoring-python)

### Batch scoring

- [Batch scoring of deep learning models](https://learn.microsoft.com/azure/architecture/reference-architectures/ai/batch-scoring-deep-learning)
- [Batch scoring of R models on Azure](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/batch-scoring-r-models)
- [Batch scoring with Spark on Databricks](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/batch-scoring-databricks)

### MLOps end-to-end

- [MLOps with Azure Machine Learning](https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-technical-paper)
- [MLOps maturity model](https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-maturity-model)
- [MLOps for Python with Azure ML](https://learn.microsoft.com/azure/architecture/reference-architectures/ai/mlops-python)

### Distributed training

- [Distributed deep learning training](https://learn.microsoft.com/azure/architecture/reference-architectures/ai/training-deep-learning)

### Data and feature engineering

- [Many models machine learning at scale](https://learn.microsoft.com/azure/architecture/ai-ml/idea/many-models-machine-learning-azure-machine-learning)
- [Forecasting at scale with Azure ML](https://learn.microsoft.com/azure/architecture/example-scenario/ai/forecasting)

### Responsible AI

- [Responsible AI dashboard](https://learn.microsoft.com/azure/machine-learning/concept-responsible-ai-dashboard)
- [Responsible AI principles](https://www.microsoft.com/ai/responsible-ai)

### Network-isolated Azure ML

- [Secure Azure ML workspace](https://learn.microsoft.com/azure/machine-learning/how-to-secure-workspace-vnet)
- [Plan for network isolation](https://learn.microsoft.com/azure/machine-learning/how-to-network-isolation-planning)
- [Azure ML behind a managed VNet](https://learn.microsoft.com/azure/machine-learning/how-to-managed-network)

### Reference architectures with related services

- [Azure ML + Synapse data prep](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/synapse-machine-learning)
- [Azure ML + Databricks](https://learn.microsoft.com/azure/architecture/ai-ml/idea/orchestrate-machine-learning-azure-databricks)
- [End-to-end personalized offers](https://learn.microsoft.com/azure/architecture/example-scenario/ai/personalized-offers)

### Well-Architected for AI workloads

- [AI workloads on Azure](https://learn.microsoft.com/azure/well-architected/ai/get-started)
- [WAF design principles for AI](https://learn.microsoft.com/azure/well-architected/ai/design-principles)

[<- Master Index](00-MASTER-INDEX.md)
