# DP-100 Visual Study Guide - Master Index

> **Designing and Implementing a Data Science Solution on Azure**
> Concept-only study aid built from the official Microsoft Learn skills measured. Diagrams, decision trees, and original summaries - no exam questions reproduced.

**Skills outline:** https://learn.microsoft.com/credentials/certifications/resources/study-guides/dp-100

---

## How to use this guide

```mermaid
flowchart LR
    A["Start Here<br/>Master Index"] --> B["Read Mind Map<br/>below"]
    B --> C["Pick a Domain<br/>1->4"]
    C --> D[Study Diagrams]
    D --> E["Memorize<br/>Decision Trees"]
    E --> F["Decision Reference<br/>final review"]
    F --> G[Exam Ready]
```

---

## The 4 Exam Domains - Mind Map

```mermaid
mindmap
  root((DP-100))
    Design and Prepare ML Solution
      Workspace Design
        Azure ML Workspace
        Resource Group Strategy
        Region Selection
        Workspace Hub vs Project
      Compute Resources
        Compute Instance
        Compute Cluster
        Serverless Compute
        Attached Compute Kubernetes
        SKU Selection
        Idle Shutdown
      Identity and Access
        Microsoft Entra ID
        Managed Identity
        RBAC roles
        Workspace MSI
      Data Assets and Datastores
        Datastore types
        Data assets URI File MLTable
        Credential vs Identity based access
        Azure Storage integration
      Environments
        Curated environments
        Custom environments
        Conda dependencies
        Docker base images
    Explore Data and Train Models
      Data Wrangling
        Pandas
        Data Wrangler
        Featurization
        Data drift detection
      Experiments and Jobs
        Command jobs
        Sweep jobs
        Pipeline jobs
        Parallel jobs
      MLflow Tracking
        Autolog
        Custom logging
        Metrics Params Artifacts
        Model registry
      Notebooks and Compute Instance
        Compute instance lifecycle
        VS Code attach
        Custom apps
      AutoML
        Classification
        Regression
        Forecasting
        Computer Vision
        NLP
        Featurization options
      Hyperparameter Tuning
        Sampling Random Grid Bayesian
        Early termination Bandit Median Truncation
        Primary metric goal
      Distributed Training
        PyTorch DDP
        TensorFlow MultiWorkerMirrored
        MPI Horovod
      Responsible AI
        RAI dashboard
        Fairness
        Error analysis
        Interpretability
        Counterfactuals
    Prepare Model for Deployment
      Model Registration
        Custom MLflow Triton
        Model versions
        Signature and conda env
      Inference Environments
        Curated for inference
        Custom scoring scripts
        score py init run
      Component and Pipeline Design
        Components
        Pipeline jobs
        Pipeline endpoints
      Validation
        Local debug
        Online endpoint local mode
        Test data slices
    Deploy and Retrain a Model
      Online Endpoints
        Managed online endpoints
        Kubernetes online endpoints
        Authentication Key Token AML token
        Deployments Blue Green
        Traffic percentage
        Mirror traffic
        Autoscaling
      Batch Endpoints
        Batch deployments
        Compute targets
        Scoring inputs and outputs
      Monitoring
        Endpoint metrics
        Application Insights
        Data collection
        Data drift monitor
        Model performance monitor
      Retraining and CI CD
        Pipeline triggers
        Schedules
        Azure DevOps GitHub Actions
        ML Ops patterns
```

---

## Official Skills Weighting

```mermaid
pie title DP-100 Skills Measured (midpoints)
  "Design and Prepare ML Solution" : 22
  "Explore Data and Train Models" : 38
  "Prepare Model for Deployment" : 22
  "Deploy and Retrain a Model" : 13
```

| Slice | Weight | Jump to chapter |
| --- | --- | --- |
| Design and Prepare ML Solution | **20-25%** | [01](01-design-and-prepare-ml-solution.md) |
| Explore Data and Train Models | **35-40%** | [02](02-explore-data-and-train-models.md) |
| Prepare Model for Deployment | **20-25%** | [03](03-prepare-model-for-deployment.md) |
| Deploy and Retrain a Model | **10-15%** | [04](04-deploy-and-retrain-model.md) |

> **Explore Data & Train Models is ~38% of the exam.** Spend 40% of your study time on AutoML, hyperparameter tuning, MLflow, and distributed training.

---

## ML lifecycle - service emphasis map

```mermaid
flowchart LR
    PLAN[Plan and Design] --> DATA[Data]
    DATA --> EXPLORE[Explore]
    EXPLORE --> TRAIN[Train]
    TRAIN --> EVAL[Evaluate]
    EVAL --> REGISTER[Register]
    REGISTER --> DEPLOY[Deploy]
    DEPLOY --> MONITOR[Monitor]
    MONITOR --> RETRAIN[Retrain]
    RETRAIN --> TRAIN

    PLAN --> P1["Workspace - Compute - Environments - RBAC"]
    DATA --> D1["Datastores - Data Assets - MLTable - Storage"]
    EXPLORE --> E1["Notebooks - Data Wrangler - pandas"]
    TRAIN --> T1["Command jobs - Sweep - AutoML - MLflow"]
    EVAL --> V1["MLflow metrics - RAI dashboard"]
    REGISTER --> R1["Model registry - MLflow / custom / Triton"]
    DEPLOY --> Y1["Online endpoints - Batch endpoints - Blue/Green"]
    MONITOR --> M1["App Insights - Data collector - Drift monitor"]
    RETRAIN --> X1["Pipeline schedules - CI/CD triggers"]
```

---

## The 6 Core Question Patterns in DP-100

```mermaid
flowchart TD
    Q[Any DP-100 question] --> P1
    Q --> P2
    Q --> P3
    Q --> P4
    Q --> P5
    Q --> P6

    P1["1 Pick the right compute<br/>(instance vs cluster vs serverless)"]
    P2["2 Pick the right job type<br/>(command vs sweep vs pipeline)"]
    P3["3 Pick the right tuning strategy<br/>(grid vs random vs Bayesian)"]
    P4["4 Pick the right endpoint<br/>(online vs batch - managed vs k8s)"]
    P5["5 Pick the right deployment swap<br/>(blue/green - traffic - mirror)"]
    P6["6 Pick the right monitoring<br/>(drift vs perf vs collection)"]

    P1 --> R1["Instance = author/dev<br/>Cluster = train at scale<br/>Serverless = no infra"]
    P2 --> R2["Command = single script<br/>Sweep = HP tuning<br/>Pipeline = multi-step DAG"]
    P3 --> R3["Bayesian = continuous + smart<br/>Random = quick baseline<br/>Grid = small discrete space"]
    P4 --> R4["Online = real-time low latency<br/>Batch = async high throughput<br/>k8s = bring your own cluster"]
    P5 --> R5["Blue/Green = two deployments<br/>Traffic % = gradual rollout<br/>Mirror = shadow new version"]
    P6 --> R6["Data drift = inputs changed<br/>Perf monitor = labels drifted<br/>Collection = log to storage"]
```

---

## The "Magic Words" Translator

```mermaid
flowchart LR
    subgraph Triggers
      T1["'no infrastructure to manage'"]
      T2["'tune hyperparameters efficiently'"]
      T3["'shadow test before promotion'"]
      T4["'detect when model goes stale'"]
      T5["'reusable training step'"]
      T6["'deploy without writing scoring script'"]
      T7["'large dataset async scoring'"]
      T8["'no stored credentials'"]
      T9["'multi-GPU training'"]
      T10["'explain a single prediction'"]
    end
    subgraph Answers
      A1["Serverless compute"]
      A2["Bayesian sampling plus Bandit early termination"]
      A3["Mirror traffic on online endpoint"]
      A4["Data drift monitor plus alerts"]
      A5["Pipeline component"]
      A6["MLflow model plus no-code deployment"]
      A7["Batch endpoint"]
      A8["Workspace managed identity plus identity-based datastore"]
      A9["Distributed PyTorch DDP or Horovod on compute cluster"]
      A10["RAI dashboard counterfactual or SHAP local"]
    end
    T1 --> A1
    T2 --> A2
    T3 --> A3
    T4 --> A4
    T5 --> A5
    T6 --> A6
    T7 --> A7
    T8 --> A8
    T9 --> A9
    T10 --> A10
```

---

## Domain files in this guide

| # | Domain | File | Focus |
|---|--------|------|-------|
| 1 | Design & Prepare ML Solution | [01-design-and-prepare-ml-solution.md](01-design-and-prepare-ml-solution.md) | Workspace, compute, datastores, environments, RBAC |
| 2 | Explore Data & Train Models | [02-explore-data-and-train-models.md](02-explore-data-and-train-models.md) | Jobs, MLflow, AutoML, hyperdrive, distributed, RAI |
| 3 | Prepare Model for Deployment | [03-prepare-model-for-deployment.md](03-prepare-model-for-deployment.md) | Model registry, scoring scripts, components, validation |
| 4 | Deploy & Retrain a Model | [04-deploy-and-retrain-model.md](04-deploy-and-retrain-model.md) | Online + batch endpoints, monitoring, retraining |
| | **Exam Decision Reference** | [05-exam-cheatsheet.md](05-exam-cheatsheet.md) | Decision trees + scenario keyword map |
| | **Concept & Reference Index** | [06-references.md](06-references.md) | Every concept linked to Microsoft Learn |
| + | **Extra Concepts** | [07-extra-dp100-concepts.md](07-extra-dp100-concepts.md) | Edge cases + ML engineering philosophy |
| + | **Microsoft Learn Summaries** | [08-learn-summaries.md](08-learn-summaries.md) | Per-service overviews |
| + | **Architectures - DP-100** | [09-arch-dp100.md](09-arch-dp100.md) | Reference Azure ML architectures |

---

## Recommended study order (13 days)

```mermaid
gantt
    title Suggested 13-day plan
    dateFormat X
    axisFormat Day %d
    section Foundations
    Design and Prepare workspace :a1, 0, 3d
    section Training
    Data plus jobs plus MLflow :b1, after a1, 2d
    AutoML plus Hyperdrive :b2, after b1, 2d
    Distributed plus RAI :b3, after b2, 1d
    section Deployment
    Prepare for deployment :c1, after b3, 3d
    Deploy plus monitor plus retrain :c2, after c1, 2d
```

---

## Quick links

- [Exam Cheatsheet](05-exam-cheatsheet.md)
- [Concept & Reference Index](06-references.md)
- [Architectures](09-arch-dp100.md)
- [Glossary](12-glossary.md)
- [Flashcards](13-flashcards.md)
- [Pitfalls](14-pitfalls.md)
- [Hands-On Labs](15-hands-on-labs.md)
- [AI Copilot Quiz](17-copilot-quiz.md)
- [Practice Assessment](99-practice-assessment.md)
