# Domain 1 - Design and Prepare an ML Solution

> **Weight: 20-25%** - Foundation domain. You design the workspace, pick compute, set up datastores and environments, and configure identity. Get this right or every later domain becomes painful.

---

## Mind map

```mermaid
mindmap
  root((01 Design and Prepare))
    Workspace
      Workspace Hub
      Project workspace
      Region
      Resource group
      Linked services Storage Key Vault App Insights ACR
    Compute
      Compute instance
      Compute cluster
      Serverless compute
      Attached compute Kubernetes Synapse
      SKU CPU GPU
      Idle shutdown
      VNet integration
    Identity and access
      Microsoft Entra ID
      Workspace managed identity SAMI UAMI
      RBAC roles
      Conditional access
    Data
      Datastores Blob ADLS Gen2 File Data Lake
      Data assets uri_file uri_folder mltable
      Credential vs identity-based access
    Environments
      Curated
      Custom
      Conda yaml
      Docker context
      Inference vs training images
    Governance
      Cost controls
      Quotas
      Tags
      Diagnostic settings
```

---

## Compute decision tree

```mermaid
flowchart TD
    Start{What are you doing?} --> Q1{Authoring code<br/>in a notebook?}
    Q1 -- Yes --> CI[Compute instance<br/>single-user dev box]
    Q1 -- No --> Q2{Train at scale<br/>or HP tuning?}
    Q2 -- Yes --> Q3{Want zero infra?}
    Q3 -- Yes --> SL[Serverless compute<br/>no quota mgmt]
    Q3 -- No --> CC[Compute cluster<br/>autoscale 0..N nodes]
    Q2 -- No --> Q4{Already have AKS<br/>or Synapse?}
    Q4 -- Yes --> ATT[Attached compute<br/>reuse existing cluster]
    Q4 -- No --> CC
```

| Compute | Use when | Min nodes | Idle behavior |
|---|---|---|---|
| **Compute instance** | Author / debug / VS Code remote | always-on (1 VM) | manual stop or scheduled idle shutdown |
| **Compute cluster** | Repeatable training, sweep, pipeline | scales to 0 | scales to 0 after `idle_seconds_before_scaledown` |
| **Serverless compute** | Quick jobs, no quota planning | n/a | autoscales transparently |
| **Attached Kubernetes (AKS Arc)** | Bring your own cluster, GPU pools you already paid for | n/a | you manage |

> **Exam trap:** A compute instance does **not** scale to 0 - leaving it running is the #1 cost surprise. Use scheduled idle shutdown.

---

## Workspace topology

```mermaid
flowchart LR
    subgraph WS[Azure ML Workspace]
      direction LR
      WSMI[(Workspace MSI)]
      MR[Model Registry]
      DA[Data Assets]
      ENV[Environments]
      JOBS[Jobs and Endpoints]
    end

    subgraph LINK[Linked resources]
      ST[(Azure Storage<br/>default datastore)]
      KV[(Key Vault)]
      AI[(Application Insights)]
      ACR[(Container Registry)]
    end

    WS --> ST
    WS --> KV
    WS --> AI
    WS --> ACR

    USR[User or Service Principal] -->|RBAC| WS
    WSMI -->|Identity-based access| ST
```

- **Workspace MSI** is the workspace's own identity. Grant it `Storage Blob Data Contributor` on the storage account if you want jobs to use identity-based access (no SAS, no keys).
- The **default datastore** (`workspaceblobstore`) is created automatically - backed by the storage account you selected at workspace creation.

---

## Datastores and data assets

```mermaid
flowchart LR
    subgraph Storage["Azure Storage Services"]
      BLOB[Blob]
      ADLS[ADLS Gen2]
      FILE[Files]
    end

    subgraph DS[Datastore = pointer + auth]
      ABL[blob datastore]
      ADL[adls_gen2 datastore]
      AFS[file datastore]
    end

    BLOB --> ABL
    ADLS --> ADL
    FILE --> AFS

    subgraph DA[Data Asset = versioned reference]
      F["uri_file<br/>(single file)"]
      FO["uri_folder<br/>(many files)"]
      ML["mltable<br/>(typed table + transforms)"]
    end

    ABL --> F & FO & ML
    ADL --> F & FO & ML
```

- **`uri_file`** - point to one CSV/parquet/etc. Used by simple command jobs.
- **`uri_folder`** - point to a folder. Used when training scripts iterate files.
- **`mltable`** - schema + load transformations described in `MLTable` YAML. Required by AutoML tabular tasks.

| Auth mode | When to pick | Trade-off |
|---|---|---|
| **Credential-based** (account key / SAS) | Quick start, sandbox | Secrets stored in workspace Key Vault - rotation pain |
| **Identity-based** (workspace MSI / user identity) | Production | Need RBAC on the storage account, cleaner audit |

---

## Environments

```mermaid
flowchart TD
    Need{Reproduce<br/>training env?}
    Need --> A{Stock framework<br/>e.g. sklearn 1.3?}
    A -- Yes --> CUR[Curated environment<br/>AzureML-...]
    A -- No --> B{Pinned versions<br/>or extra deps?}
    B -- Yes --> CONDA[Custom env<br/>conda.yaml]
    B -- No --> DOCK[Custom env<br/>Docker context build]
```

- **Curated** envs = Microsoft-maintained, signed, published to ACR. Fastest startup. Names start with `AzureML-`.
- **Custom** envs = your conda spec or Dockerfile. Built once, cached in the workspace ACR.
- Always **pin** versions for reproducibility. `azureml-mlflow` and `mlflow` should match.

---

## Identity and RBAC

```mermaid
flowchart TD
    User[User or SP] --> Role{Workspace role}
    Role --> R1[AzureML Data Scientist<br/>run jobs, register models]
    Role --> R2[AzureML Compute Operator<br/>manage compute only]
    Role --> R3[Reader<br/>view, no run]
    Role --> R4[Contributor<br/>everything except role assignment]
    Role --> R5[Owner<br/>full]
```

> **Least privilege rule:** Data Scientist role + storage RBAC is enough for 90% of users. Avoid Owner outside platform team.

---

## Networking quick view

```mermaid
flowchart LR
    INET[Public Internet] -.->|public access| WS
    PE[Private Endpoint] --> WS[(Azure ML Workspace)]
    WS --> SUB[VNet Subnet]
    SUB --> CC[Compute cluster<br/>VNet-injected]
    SUB --> ST[(Storage<br/>private endpoint)]
    SUB --> KV[(Key Vault<br/>private endpoint)]
```

For a fully private workspace: private endpoints on workspace + storage + Key Vault + ACR + App Insights, VNet-injected compute, and `public_network_access_enabled = false`.

---

## Common pitfalls

- **Compute instance left running** overnight - use idle shutdown.
- **Curated environment doesn't match your pinned package** - switch to a custom env.
- **Identity-based datastore but no RBAC** - workspace MSI needs `Storage Blob Data Contributor`.
- **Wrong region** for GPU SKUs - check quota + availability before workspace create.
- **Region mismatch** between workspace and storage causes egress cost + latency.
- **AutoML expects MLTable** - `uri_folder` will fail with cryptic errors.

---

## Microsoft Learn

- [What is an Azure Machine Learning workspace?](https://learn.microsoft.com/azure/machine-learning/concept-workspace)
- [Azure ML compute targets](https://learn.microsoft.com/azure/machine-learning/concept-compute-target)
- [Manage data assets](https://learn.microsoft.com/azure/machine-learning/how-to-create-data-assets)
- [Azure ML environments](https://learn.microsoft.com/azure/machine-learning/concept-environments)
- [Built-in roles for Azure ML](https://learn.microsoft.com/azure/machine-learning/how-to-assign-roles)

---

[<- Master Index](00-MASTER-INDEX.md) - [Explore Data and Train Models ->](02-explore-data-and-train-models.md)
