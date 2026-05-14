# Hands-On Labs and Sample Repositories

> Curated, executable references for DP-100 topics. All links point to official Microsoft Learn modules, Azure-Samples repositories, or Cloud Adoption Framework / Well-Architected Framework assessments.

## Microsoft Learn - Sandbox-Backed Lab Series

> Repo: <https://github.com/MicrosoftLearning/mslearn-azureml> - Site: <https://microsoftlearning.github.io/mslearn-azureml/>

| # | Lab | Objective covers |
|---|---|---|
| 1 | Explore Azure Machine Learning workspace resources and tools | Domain 1 |
| 2 | Make data available in Azure Machine Learning | Domain 1 |
| 3 | Work with compute resources in Azure Machine Learning | Domain 1 |
| 4 | Work with environments in Azure Machine Learning | Domain 1 |
| 5 | Find the best classification model with Automated ML | Domain 2 |
| 6 | Track model training in Jupyter notebooks with MLflow | Domain 2 |
| 7 | Run a training script as a command job | Domain 2 |
| 8 | Use MLflow to track training jobs | Domain 2 |
| 9 | Perform hyperparameter tuning with a sweep job | Domain 2 |
| 10 | Run pipelines | Domain 2 |
| 11 | Log and register models | Domain 3 |
| 12 | Deploy a model to an online endpoint | Domain 4 |
| 13 | Deploy a model to a batch endpoint | Domain 4 |

## Reinforcement examples (azureml-examples)

> Repo: <https://github.com/Azure/azureml-examples>

- `cli/jobs/single-step/` - minimal command job
- `cli/jobs/sweep/` - sweep with Bandit policy
- `cli/jobs/pipelines/` - multi-component pipeline
- `cli/endpoints/online/managed/` - blue/green online deployment
- `cli/endpoints/batch/` - batch endpoint with parallel scoring
- `sdk/python/jobs/automl-standalone-jobs/` - AutoML classification/regression/forecasting
- `sdk/python/responsible-ai/` - RAI dashboard

## Concept playgrounds

- [MLflow tracking quickstart](https://learn.microsoft.com/azure/machine-learning/quickstart-run-notebooks)
- [Train and deploy with VS Code Azure ML extension](https://learn.microsoft.com/azure/machine-learning/how-to-set-up-vs-code)
- [Work with the CLI v2](https://learn.microsoft.com/azure/machine-learning/how-to-train-model)

## Exam-tier challenges (build it yourself)

1. Build a 3-step pipeline (prep / train / register) using YAML components.
2. Run a sweep with Random sampling + Bandit early-termination over 4 HPs; verify it picks the best by primary metric.
3. Deploy an MLflow model to an online endpoint with **blue + green** and shift 10% mirror traffic to green.
4. Deploy a model to a batch endpoint and score 1000 CSV files in parallel.
5. Enable data collector on the online endpoint, then set up a drift + performance monitor.
6. Wire GitHub Actions with **OIDC** to register a model on every PR merge.
7. Lock down a workspace with **private endpoints + VNet-injected compute**.

---

[<- Master Index](00-MASTER-INDEX.md)
