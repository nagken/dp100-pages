# Flashcards: Active Recall

> Click any card to reveal the answer. Use the **Domain pager bottom-right** to switch between exam areas. ~50 cards across 4 domains.

<section class="fc-section" data-fc-title="Design and Prepare ML Solution">
<h2>1 - Design and Prepare ML Solution</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">"No infrastructure to manage" -> which compute?</div><div class="fc-a"><strong>Serverless compute</strong> - Azure ML provisions, scales, and tears down for you.</div></div>

<div class="flashcard"><div class="fc-q">Compute instance vs compute cluster?</div><div class="fc-a">Instance = single-VM authoring (notebooks, dev). Cluster = multi-node training, scales <code>min_instances=0</code> -> N.</div></div>

<div class="flashcard"><div class="fc-q">Compute instance scales to 0?</div><div class="fc-a">No - it's a single VM. Use <strong>idle shutdown</strong> to control cost.</div></div>

<div class="flashcard"><div class="fc-q">Datastore auth without stored credentials?</div><div class="fc-a"><strong>Identity-based access</strong> (workspace managed identity). No keys/SAS in YAML.</div></div>

<div class="flashcard"><div class="fc-q">AutoML on tabular data needs which data asset?</div><div class="fc-a"><strong>MLTable</strong> - required for AutoML and parallel jobs.</div></div>

<div class="flashcard"><div class="fc-q">Fully private workspace checklist?</div><div class="fc-a">Private endpoints + VNet-injected compute + <code>public_network_access_enabled=false</code> + private DNS zones.</div></div>

<div class="flashcard"><div class="fc-q">Curated env doesn't have my pinned package - what now?</div><div class="fc-a">Build a <strong>custom environment</strong> (conda spec + Docker base image).</div></div>

<div class="flashcard"><div class="fc-q">Data Scientist role for jobs + models, not Owner?</div><div class="fc-a"><strong>AzureML Data Scientist</strong> RBAC role.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Explore Data and Train Models">
<h2>2 - Explore Data and Train Models</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Multi-step training workflow?</div><div class="fc-a"><strong>Pipeline job</strong> composed of components (reusable, versioned).</div></div>

<div class="flashcard"><div class="fc-q">Hyperparameter search job type?</div><div class="fc-a"><strong>Sweep</strong> job - wraps a command job with sampling + termination policy.</div></div>

<div class="flashcard"><div class="fc-q">Tune HPs on continuous params, no early termination?</div><div class="fc-a"><strong>Bayesian sampling</strong> - must run to completion.</div></div>

<div class="flashcard"><div class="fc-q">Bayesian sampling + Bandit policy?</div><div class="fc-a"><strong>INVALID</strong> - Bayesian sampling does not allow early termination.</div></div>

<div class="flashcard"><div class="fc-q">Bandit policy - what does it do?</div><div class="fc-a">Aggressively kills runs outside <code>slack_factor</code> of the best run so far.</div></div>

<div class="flashcard"><div class="fc-q">Map function over partitions of data?</div><div class="fc-a"><strong>Parallel job</strong> - maps mini-batches across many workers.</div></div>

<div class="flashcard"><div class="fc-q">Multi-GPU multi-node training?</div><div class="fc-a"><code>distribution.type</code> = <code>pytorch</code> (DDP) or <code>mpi</code> (Horovod).</div></div>

<div class="flashcard"><div class="fc-q">AutoML featurization off - when?</div><div class="fc-a">When you've already engineered features upstream.</div></div>

<div class="flashcard"><div class="fc-q">AutoML forecasting requires?</div><div class="fc-a"><code>time_column_name</code> + MLTable data asset.</div></div>

<div class="flashcard"><div class="fc-q">Track training experiments with autologging?</div><div class="fc-a"><strong>MLflow</strong> - <code>mlflow.autolog()</code> + Azure ML tracking URI.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Prepare Model for Deployment">
<h2>3 - Prepare Model for Deployment</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Deploy without scoring script - what model type?</div><div class="fc-a"><strong>MLflow model</strong> (<code>mlflow_model</code>) - Azure ML auto-generates score.py.</div></div>

<div class="flashcard"><div class="fc-q">Custom inference env requires which package?</div><div class="fc-a"><code>azureml-inference-server-http</code> - required for custom (non-MLflow) deployments.</div></div>

<div class="flashcard"><div class="fc-q">Where does score.py find the model?</div><div class="fc-a"><code>$AZUREML_MODEL_DIR</code> environment variable.</div></div>

<div class="flashcard"><div class="fc-q">score.py init() vs run()?</div><div class="fc-a"><strong>init()</strong> = load model once at startup. <strong>run()</strong> = called per request.</div></div>

<div class="flashcard"><div class="fc-q">Async score 10M records - which endpoint?</div><div class="fc-a"><strong>Batch endpoint</strong> - high-throughput, async, file-in/file-out.</div></div>

<div class="flashcard"><div class="fc-q">Real-time low-latency scoring - which endpoint?</div><div class="fc-a"><strong>Managed online endpoint</strong> - sync HTTP, autoscale, blue/green.</div></div>

<div class="flashcard"><div class="fc-q">Shadow test new model - which traffic mode?</div><div class="fc-a"><strong>Mirror traffic</strong> (<=50%) - sends a copy without affecting prod responses.</div></div>

<div class="flashcard"><div class="fc-q">Blue/green deployment - what changes?</div><div class="fc-a"><code>traffic</code> percentages between deployments under one endpoint.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Deploy and Retrain a Model">
<h2>4 - Deploy and Retrain a Model</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Detect input distribution shift?</div><div class="fc-a"><strong>Data drift monitor</strong> on input features (production vs reference dataset).</div></div>

<div class="flashcard"><div class="fc-q">Compare predictions to ground truth post-prod?</div><div class="fc-a"><strong>Model performance monitor</strong> - needs labeled feedback joined back to predictions.</div></div>

<div class="flashcard"><div class="fc-q">Explain a single prediction - RAI tool?</div><div class="fc-a"><strong>Counterfactual</strong> or <strong>local SHAP</strong>.</div></div>

<div class="flashcard"><div class="fc-q">RAI dashboard components?</div><div class="fc-a">Error analysis, fairness, SHAP/feature importance, counterfactual, causal.</div></div>

<div class="flashcard"><div class="fc-q">OIDC from GitHub Actions - what removes?</div><div class="fc-a"><strong>Stored secrets</strong> - uses workload identity federation.</div></div>

<div class="flashcard"><div class="fc-q">Online endpoint scaling modes?</div><div class="fc-a">Manual (fixed <code>instance_count</code>) or Autoscale (Azure Monitor rules on CPU/RPS/custom).</div></div>

<div class="flashcard"><div class="fc-q">Endpoint unhealthy - first thing to check?</div><div class="fc-a">Container logs (<code>az ml online-deployment get-logs</code>) - usually init() crash from missing package or wrong model path.</div></div>

<div class="flashcard"><div class="fc-q">Cost: cheapest way to run training cluster?</div><div class="fc-a"><strong>Spot / low-priority VMs</strong> + <code>min_instances=0</code> + early-termination policy.</div></div>

</div>
</section>
