# Domain 2 - Explore Data and Train Models

> **Weight: 35-40%** - The biggest domain. Jobs, MLflow tracking, AutoML, hyperparameter tuning, distributed training, and Responsible AI. Master this.

---

## Mind map

```mermaid
mindmap
  root((02 Train))
    Data exploration
      Data Wrangler
      pandas
      Notebooks
      Featurization
    Job types
      Command job
      Sweep job hyperdrive
      Pipeline job
      Parallel job
    AutoML
      Classification
      Regression
      Forecasting
      Computer Vision
      NLP
      Featurization
      Primary metric
      Best run selection
    Hyperparameter tuning
      Sampling random grid bayesian
      Early termination bandit median truncation
      Primary metric goal
      Concurrent runs
    MLflow
      mlflow.autolog
      log_metric log_param log_artifact
      Model registry
      Run hierarchy
    Distributed training
      PyTorch DDP
      TensorFlow MultiWorkerMirrored
      MPI Horovod
      Process count per node
    Responsible AI
      RAI dashboard
      Fairness FairLearn
      Error analysis
      Interpretability SHAP mimic
      Counterfactuals
      Causal analysis
```

---

## Job decision tree

```mermaid
flowchart TD
    Start{What are you running?} --> Q1{Single script<br/>one config?}
    Q1 -- Yes --> CMD[Command job<br/>type: command]
    Q1 -- No --> Q2{Tuning hyperparameters?}
    Q2 -- Yes --> SW[Sweep job<br/>type: sweep<br/>wraps a command job]
    Q2 -- No --> Q3{Multi-step DAG<br/>data prep then train then register?}
    Q3 -- Yes --> PIPE[Pipeline job<br/>type: pipeline<br/>composed of components]
    Q3 -- No --> Q4{Map a function<br/>over many partitions?}
    Q4 -- Yes --> PAR[Parallel job<br/>type: parallel]
```

| Job | YAML `type` | Inputs | Use case |
|---|---|---|---|
| Command | `command` | one script + args | "train.py with these params" |
| Sweep | `sweep` | command job + search space + sampler + termination | hyperparameter tuning |
| Pipeline | `pipeline` | components stitched via outputs->inputs | multi-stage workflow |
| Parallel | `parallel` | partitioned data + entry script | embarrassingly parallel inference / featurization |

---

## Hyperparameter sweep - pick the strategy

```mermaid
flowchart TD
    Q1{Is the search space<br/>small and discrete?}
    Q1 -- Yes --> GRID[Grid sampling<br/>exhaustive]
    Q1 -- No --> Q2{Need quick baseline<br/>or large space?}
    Q2 -- Yes --> RAND[Random sampling<br/>fast and parallel]
    Q2 -- No --> Q3{Continuous params<br/>and you can wait?}
    Q3 -- Yes --> BAY[Bayesian sampling<br/>smart sequential]
    Q3 -- No --> RAND
```

**Sampling rules:**

- **Grid** - `choice()` only. Fully enumerates. Use when you have <= ~50 combos.
- **Random** - works with `choice`, `uniform`, `loguniform`, `normal`, etc. Best general-purpose default.
- **Bayesian** - picks each next config based on past results. **Cannot use early termination policies.** Continuous distributions only (`uniform`, `quniform`, `choice`).

**Early termination policies** (kill bad runs early - saves cost):

| Policy | Decision rule | Picks |
|---|---|---|
| **Bandit** | Kill if metric within `slack_factor` of best | Aggressive - quick saver |
| **Median Stopping** | Kill if running average worse than median of completed runs at same interval | Conservative |
| **Truncation Selection** | Kill bottom `truncation_percentage` at each evaluation interval | Predictable cull |
| **No policy** | All runs complete | Bayesian only |

---

## MLflow flow

```mermaid
flowchart LR
    SCR[train.py] -->|mlflow.autolog or<br/>mlflow.log_metric| TR[MLflow Tracking<br/>built into AML]
    TR --> RUN[Job / run]
    RUN --> METR[Metrics]
    RUN --> PAR[Params]
    RUN --> ART[Artifacts]
    RUN --> MOD[Model]
    MOD -->|register_model| REG[Model Registry]
    REG --> DEPLOY[Deployment to endpoint]
```

- **`mlflow.autolog()`** - captures sklearn / pytorch / xgboost / tensorflow params + metrics + model **automatically**. First call you should make.
- An **MLflow model** has `MLmodel` + `conda.yaml` + `model.pkl` (or framework files). Required for **no-code deployment** to online endpoint.
- Azure ML's tracking URI is set automatically inside a job; outside a job, use `azureml.mlflow` to get the URI from the workspace.

---

## AutoML decision matrix

```mermaid
flowchart TD
    Task{Task} --> CL[Classification]
    Task --> RE[Regression]
    Task --> FC[Forecasting]
    Task --> CV[Computer Vision]
    Task --> NL[NLP]

    CL --> CLM[Primary metrics:<br/>accuracy - AUC_weighted - norm_macro_recall - avg_precision_score_weighted]
    RE --> REM[Primary metrics:<br/>r2_score - normalized_root_mean_squared_error - spearman_correlation]
    FC --> FCM[Primary metrics:<br/>normalized_root_mean_squared_error - normalized_mean_absolute_error]
```

- **Featurization** modes: `auto` (default), `off`, `custom` (provide a `featurization_config`).
- **Forecasting** needs `time_column_name` and optionally `time_series_id_column_names` for multi-series.
- **Cross-validation**: set `n_cross_validations` (or use a validation dataset).
- **Best run** is auto-selected by primary metric; child runs are tracked in MLflow.

---

## Distributed training

```mermaid
flowchart LR
    CC[Compute cluster<br/>4 nodes x 4 GPUs] --> DIST{distribution.type}
    DIST --> PT[pytorch<br/>process_count_per_instance]
    DIST --> TF[tensorflow<br/>worker_count + parameter_server_count]
    DIST --> MPI[mpi<br/>process_count_per_instance + Horovod]
    PT --> NCC[NCCL backend on GPU]
    MPI --> HOR[Horovod allreduce]
```

- **PyTorch DDP** - `distribution: type: pytorch` and set `process_count_per_instance` to GPUs per node.
- **TensorFlow** - `MultiWorkerMirroredStrategy` typically.
- **Horovod** - runs as MPI; works with TF, PyTorch, MXNet.
- Cluster needs an **InfiniBand-enabled SKU** (`NDv2`, `ND_A100_v4`) for efficient multi-GPU.

---

## Responsible AI dashboard

```mermaid
flowchart LR
    M[Trained Model] --> RAI[RAI Dashboard]
    RAI --> EA[Error Analysis<br/>where does the model fail?]
    RAI --> FA[Fairness<br/>across sensitive features]
    RAI --> IN[Interpretability<br/>SHAP global and local]
    RAI --> CF[Counterfactuals<br/>what change flips the prediction?]
    RAI --> CA[Causal Analysis<br/>treatment effect]
```

- Build with the **`Responsible AI Insights` component** in a pipeline, then add specific tool components (Error Analysis, Counterfactual, Causal, Explanation).
- The dashboard renders inside the studio under the registered model's "Responsible AI" tab.

---

## Compute instance vs cluster - when training

```mermaid
flowchart LR
    CI[Compute instance] -->|interactive notebook| EXP[Quick experiments]
    CC[Compute cluster] -->|batched submitted job| TRAIN[Production training]
    SL[Serverless compute] -->|no infra| BOTH[Either]
```

- A compute instance can run jobs for prototyping but doesn't autoscale. For sweep / pipeline / parallel jobs you want a cluster or serverless.

---

## Common pitfalls

- **Bayesian sampling + early termination** = invalid. Bayesian must run to completion.
- **Grid + continuous distribution** = invalid. Grid requires `choice()` only.
- **AutoML forecasting without time column** = error.
- **Forgot to set primary metric goal** (`maximize` vs `minimize`) - sweep picks wrong best run.
- **Autolog logs nothing** because the framework version isn't supported - pin compatible versions.
- **Distributed PyTorch but `process_count_per_instance = 1`** - you're running one process; no gradient sync.
- **Compute cluster has min nodes > 0** - costs money 24/7. Default to `min_instances=0`.
- **Concurrent runs > cluster max_instances** - runs queue, sweep slows down.

---

## Microsoft Learn

- [Train models with Azure Machine Learning](https://learn.microsoft.com/azure/machine-learning/concept-train-machine-learning-model)
- [Hyperparameter tuning a model](https://learn.microsoft.com/azure/machine-learning/how-to-tune-hyperparameters)
- [What is automated ML?](https://learn.microsoft.com/azure/machine-learning/concept-automated-ml)
- [Track ML experiments and models with MLflow](https://learn.microsoft.com/azure/machine-learning/how-to-use-mlflow-cli-runs)
- [Distributed GPU training guide](https://learn.microsoft.com/azure/machine-learning/how-to-train-distributed-gpu)
- [Responsible AI dashboard](https://learn.microsoft.com/azure/machine-learning/concept-responsible-ai-dashboard)

---

[<- Design and Prepare ML Solution](01-design-and-prepare-ml-solution.md) - [Prepare Model for Deployment ->](03-prepare-model-for-deployment.md)
