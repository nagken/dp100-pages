# Practice DP-100 with Microsoft Copilot

> Microsoft Copilot is **free** at [copilot.microsoft.com](https://copilot.microsoft.com). Pick a quiz mode below - the prompt is **copied to your clipboard automatically**, Copilot opens in a new tab, just **paste (Ctrl+V) and press Enter**.

> [!TIP]
> Stay in the same chat tab for a full session - Copilot keeps score and tracks your weak topics. Click another card to start a fresh quiz.

---

## Choose a quiz mode

<style>
.qz-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 16px; margin: 22px 0 8px; }
.qz-card { position: relative; display: block; padding: 22px 22px 56px; border-radius: 4px; border: 1px solid #e1dfdd; background: #ffffff; color: #1b1b1b !important; text-decoration: none !important; transition: border-color .15s ease, box-shadow .15s ease, transform .15s ease; cursor: pointer; min-height: 150px; }
.qz-card:hover { border-color: #0f6cbd; box-shadow: 0 8px 24px rgba(15,108,189,.18), 0 2px 4px rgba(0,0,0,.06); transform: translateY(-2px); }
.qz-card .qz-eyebrow { display: block; font-size: .72rem; font-weight: 600; letter-spacing: .08em; text-transform: uppercase; color: #0f6cbd; margin-bottom: 8px; }
.qz-card h3 { margin: 0 0 8px; font-size: 1.15rem; font-weight: 600; color: #1b1b1b !important; line-height: 1.3; }
.qz-card p { margin: 0; font-size: .92rem; line-height: 1.5; color: #424242; }
.qz-card .qz-cta { position: absolute; left: 22px; bottom: 18px; font-size: .85rem; font-weight: 600; color: #0f6cbd; letter-spacing: .01em; }
.qz-card .qz-cta::after { content: " \2192"; transition: margin-left .15s ease; }
.qz-card:hover .qz-cta::after { margin-left: 4px; }
.qz-card.qz-flash { animation: qzFlash .6s ease; }
@keyframes qzFlash { 0% { background:#fff; } 30% { background:#dff6dd; border-color:#107c10; } 100% { background:#fff; } }
.qz-toast { position: fixed; right: 24px; bottom: 24px; background: #107c10; color: #fff; padding: 12px 18px; border-radius: 4px; font-size: .9rem; font-weight: 500; box-shadow: 0 8px 24px rgba(0,0,0,.2); opacity: 0; transform: translateY(8px); transition: opacity .2s ease, transform .2s ease; pointer-events: none; z-index: 9999; }
.qz-toast.show { opacity: 1; transform: translateY(0); }
.qz-alt { display: flex; gap: 10px; flex-wrap: wrap; margin: 18px 0 0; }
.qz-alt button { display: inline-flex; align-items: center; gap: 8px; padding: 10px 16px; border-radius: 4px; border: 1px solid #e1dfdd; background: #fff; color: #1b1b1b; font-size: .9rem; font-weight: 500; cursor: pointer; font-family: inherit; transition: border-color .15s ease, background .15s ease; }
.qz-alt button:hover { border-color: #0f6cbd; background: #f3f9fd; color: #0f6cbd; }
body:not(.theme-light) .qz-card { background: #2d2d2d; color: #f3f2f1 !important; border-color: rgba(255,255,255,.10); }
body:not(.theme-light) .qz-card h3 { color: #fff !important; }
body:not(.theme-light) .qz-card p { color: #d2d0ce; }
body:not(.theme-light) .qz-card .qz-eyebrow { color: #50e6ff; }
body:not(.theme-light) .qz-card .qz-cta { color: #50e6ff; }
body:not(.theme-light) .qz-card:hover { border-color: #50e6ff; box-shadow: 0 8px 24px rgba(80,230,255,.18); }
body:not(.theme-light) .qz-card.qz-flash { animation: qzFlashDark .6s ease; }
@keyframes qzFlashDark { 0% { background:#2d2d2d; } 30% { background:#0e3a16; border-color:#107c10; } 100% { background:#2d2d2d; } }
body:not(.theme-light) .qz-alt button { background: #2d2d2d; color: #f3f2f1; border-color: rgba(255,255,255,.18); }
body:not(.theme-light) .qz-alt button:hover { background: #383838; color: #50e6ff; border-color: #50e6ff; }
</style>

<div class="qz-grid" id="qzGrid">
  <a class="qz-card" data-prompt="You are a Microsoft DP-100 certification practice-exam coach. Quiz me one question at a time, exam-style (multiple choice or scenario). After I answer, tell me if I am right, give the correct answer, and a 2-3 line explanation tied to official Microsoft Learn. Cover all DP-100 domains: Design and prepare a machine learning solution (workspace, compute - instance/cluster/serverless/k8s, datastores, data assets, environments, RBAC); Explore data and train models (command/sweep/pipeline/parallel jobs, sampling - Grid/Random/Bayesian, early termination - Bandit/Median/Truncation, MLflow tracking, AutoML, distributed training, Responsible AI dashboard); Prepare a model for deployment (mlflow_model vs custom_model, scoring scripts, inference environments, components and pipelines); Deploy and retrain a model (managed online vs k8s online vs batch endpoints, blue/green and mirror traffic, auth modes, data collector, drift and performance monitors, MLOps with GitHub Actions OIDC). Do not show the answer until I commit. Start with question 1 now." data-target="copilot" href="#"><span class="qz-eyebrow">Full exam mode</span><h3>Sequential Practice Exam</h3><p>Exam-style questions across every DP-100 domain. One at a time, with a Microsoft Learn-grounded explanation after each answer.</p><span class="qz-cta">Copy prompt and open Copilot</span></a>
  <a class="qz-card" data-prompt="Microsoft DP-100 rapid-fire mode: ask me 20 short multiple-choice questions, one at a time, covering Azure Machine Learning workspace, compute, data assets, jobs, sweep sampling, early termination, MLflow, AutoML, online/batch endpoints, deployment strategies, monitoring, and MLOps. After each I answer, give one-line correctness and the right answer. At the end give my score and the 3 weakest topics with Microsoft Learn links to study. Begin." data-target="copilot" href="#"><span class="qz-eyebrow">Speed round</span><h3>Rapid-Fire (20 questions)</h3><p>Twenty short multiple-choice items back-to-back. Final score plus a ranked list of your three weakest topics with study links.</p><span class="qz-cta">Copy prompt and open Copilot</span></a>
  <a class="qz-card" data-prompt="You are a Microsoft DP-100 coach. Drill me on Azure ML decision trees: which compute (instance vs cluster vs serverless vs k8s), which job type (command vs sweep vs pipeline vs parallel), which sampling (Grid vs Random vs Bayesian), which early-termination policy (Bandit vs Median vs Truncation), which endpoint (managed online vs k8s online vs batch), which model flavor (mlflow_model vs custom_model vs triton_model), which deployment strategy (blue-green vs canary vs mirror). Give a real-world scenario and 4 plausible options. Wait for my answer, then explain why the right option wins and why the others are traps. Begin." data-target="copilot" href="#"><span class="qz-eyebrow">Service selection</span><h3>Decision-Tree Drills</h3><p>"Which Azure ML option?" scenarios with four plausible answers. Distractors explained, not just the right one.</p><span class="qz-cta">Copy prompt and open Copilot</span></a>
  <a class="qz-card" data-prompt="You are a Microsoft DP-100 coach. Quiz me on the 50 most common 'gotchas' and trick distinctions on the exam: compute instance does NOT scale to 0; AutoML tabular requires MLTable not uri_folder; Bayesian sampling does NOT support early termination; mirror traffic capped at 50%; mlflow_model enables no-code deploy but custom_model does not; data collector required before drift monitor; identity-based access vs credential-based; serverless vs cluster trade-offs; AZUREML_MODEL_DIR; SDK v1 vs v2 API differences. One at a time, multiple choice, then explanation. Begin." data-target="copilot" href="#"><span class="qz-eyebrow">Trick questions</span><h3>Gotcha and Trap Drill</h3><p>The 50 most-confused exam distinctions: SKU traps, sampling/policy invalid combos, deploy edge cases.</p><span class="qz-cta">Copy prompt and open Copilot</span></a>
  <a class="qz-card" data-prompt="You are a Microsoft DP-100 architecture coach. Give me one Azure Machine Learning design scenario at a time (org constraints, data volume, latency target, compliance). I will propose a solution (compute choice, data asset type, job + pipeline shape, deployment endpoint, monitoring + retraining loop, identity model). You critique it against the Well-Architected Framework (Cost, Reliability, Security, Operations, Performance) and give the official-pattern answer with Microsoft Learn references. Begin." data-target="copilot" href="#"><span class="qz-eyebrow">Design scenarios</span><h3>Architecture Critique</h3><p>You propose an MLOps architecture; Copilot grades it against the Well-Architected Framework and gives the official-pattern answer.</p><span class="qz-cta">Copy prompt and open Copilot</span></a>
  <a class="qz-card" data-prompt="I just took the DP-100 practice exam. First ask me which 2 domains I want to drill (Design and prepare ML solution / Explore data and train models / Prepare model for deployment / Deploy and retrain model). Then ask 10 deeper scenario questions on those domains with detailed explanations referencing Microsoft Learn. Begin by asking which domains." data-target="copilot" href="#"><span class="qz-eyebrow">Targeted study</span><h3>Weak-Area Deep Dive</h3><p>Tell Copilot the two DP-100 domains you struggle with and get ten deeper scenario questions on those areas only.</p><span class="qz-cta">Copy prompt and open Copilot</span></a>
</div>

<div class="qz-alt">
  <button data-prompt="You are a Microsoft DP-100 certification practice-exam coach. Quiz me one question at a time, exam-style. After I answer, give correctness, the right answer, and a 2-3 line Microsoft Learn-grounded explanation. Cover all DP-100 domains: workspace + compute + data + environments + RBAC; command/sweep/pipeline/parallel jobs, MLflow, AutoML, distributed training, Responsible AI dashboard; mlflow_model vs custom_model, scoring scripts, inference environments; managed online vs k8s online vs batch endpoints, blue/green + mirror traffic, data collector, drift + perf monitors, MLOps with OIDC. Do not show the answer until I commit. Start with question 1." data-target="gemini">Open same prompt in Google Gemini</button>
  <button data-prompt="You are a Microsoft DP-100 certification practice-exam coach. Quiz me one question at a time, exam-style. After I answer, give correctness, the right answer, and a 2-3 line Microsoft Learn-grounded explanation. Cover all DP-100 domains: workspace + compute + data + environments + RBAC; command/sweep/pipeline/parallel jobs, MLflow, AutoML, distributed training, Responsible AI dashboard; mlflow_model vs custom_model, scoring scripts, inference environments; managed online vs k8s online vs batch endpoints, blue/green + mirror traffic, data collector, drift + perf monitors, MLOps with OIDC. Do not show the answer until I commit. Start with question 1." data-target="chatgpt">Open same prompt in ChatGPT</button>
  <button data-prompt="You are a Microsoft DP-100 certification practice-exam coach. Quiz me one question at a time, exam-style. After I answer, give correctness, the right answer, and a 2-3 line Microsoft Learn-grounded explanation. Cover all DP-100 domains: workspace + compute + data + environments + RBAC; command/sweep/pipeline/parallel jobs, MLflow, AutoML, distributed training, Responsible AI dashboard; mlflow_model vs custom_model, scoring scripts, inference environments; managed online vs k8s online vs batch endpoints, blue/green + mirror traffic, data collector, drift + perf monitors, MLOps with OIDC. Do not show the answer until I commit. Start with question 1." data-target="claude">Open same prompt in Claude</button>
</div>

<div class="qz-toast" id="qzToast">Prompt copied. Paste with Ctrl+V in Copilot.</div>

<script>
(function () {
  var TARGETS = { copilot: 'https://copilot.microsoft.com/', gemini: 'https://gemini.google.com/app', chatgpt: 'https://chat.openai.com/', claude: 'https://claude.ai/new' };
  var toast = document.getElementById('qzToast');
  function showToast(msg) { if (!toast) return; toast.textContent = msg; toast.classList.add('show'); setTimeout(function () { toast.classList.remove('show'); }, 2400); }
  function fallbackCopy(text) { var ta = document.createElement('textarea'); ta.value = text; ta.style.position = 'fixed'; ta.style.opacity = '0'; document.body.appendChild(ta); ta.select(); try { document.execCommand('copy'); } catch (e) {} document.body.removeChild(ta); }
  function copyText(text) { if (navigator.clipboard && navigator.clipboard.writeText) { return navigator.clipboard.writeText(text).catch(function () { fallbackCopy(text); }); } fallbackCopy(text); return Promise.resolve(); }
  function handle(el, ev) { ev.preventDefault(); var prompt = el.getAttribute('data-prompt') || ''; var target = el.getAttribute('data-target') || 'copilot'; var url = TARGETS[target] || TARGETS.copilot; copyText(prompt).then(function () { showToast('Prompt copied. Paste with Ctrl+V in the chat box.'); el.classList.add('qz-flash'); setTimeout(function () { el.classList.remove('qz-flash'); }, 600); window.open(url, '_blank', 'noopener'); }); }
  document.querySelectorAll('.qz-card, .qz-alt button').forEach(function (el) { el.addEventListener('click', function (e) { handle(el, e); }); });
})();
</script>

---

## Tips for the best practice session

1. **Stay in one chat** for a full quiz - Copilot tracks score and weak topics.
2. After 5-10 questions, type `score me so far` for a running tally.
3. Type `harder, scenario-based` or `simpler, recall-only` to retune difficulty.
4. To target one domain only, type `only ask about <domain name>`.
5. Type `cite the Microsoft Learn page` after any answer for the official source link.

---

## Free LLM options compared

| Tool | Free tier | Best for |
| --- | --- | --- |
| **Microsoft Copilot** ([copilot.microsoft.com](https://copilot.microsoft.com)) | Unlimited chats with any Microsoft account | Recommended starting point - grounded in current Microsoft Learn content. |
| **Google Gemini** ([gemini.google.com](https://gemini.google.com)) | Free tier with any Google account | Optional second opinion when you want an additional perspective. |
| **ChatGPT** ([chat.openai.com](https://chat.openai.com)) | Free tier | Optional second opinion for alternate explanations. |
| **Claude** ([claude.ai](https://claude.ai)) | Free tier | Optional second opinion for alternate explanations. |

> [!IMPORTANT]
> [Microsoft Learn](https://learn.microsoft.com) is the authoritative source for DP-100 content. Use any LLM as a study aid and always confirm specific service limits, region availability, and pricing on the official Microsoft Learn pages linked throughout this guide.

[<- Master Index](00-MASTER-INDEX.md)
