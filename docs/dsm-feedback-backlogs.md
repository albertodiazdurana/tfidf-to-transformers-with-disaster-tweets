# DSM Feedback Log - Disaster Tweet Classification

**Project:** Sprint 3 - NLP Text Classification
**DSM Version:** v1.3.7
**Purpose:** Track methodology effectiveness and collect improvement suggestions

---

## Feedback Format

Each entry should include:
- **Date:** YYYY-MM-DD
- **Checkpoint:** Day X
- **DSM Section Referenced:** (e.g., Section 2.2, Appendix D.2)
- **What Worked Well:**
- **What Was Unclear/Missing:**
- **Suggestion for DSM Improvement:**

---

## Feedback Entries

### Day 0 - Project Setup (2026-01-26)

**DSM Sections Referenced:**
- DSM_0 Section 4 (Quick Start Guide)
- DSM_2.0 PM Guidelines (Templates)
- DSM_3 Implementation Guide (CLAUDE.md template)
- Appendix D.2 (NLP Domain)

**What Worked Well:**
- Step-by-step Quick Start Guide provided clear setup sequence
- PM Guidelines templates (Daily Breakdown, MUST/SHOULD/COULD) useful for planning
- Backlog system allowed capturing improvement ideas during setup
- CLAUDE.md template with @import syntax kept configuration clean
- Checkpoint template structure provides good progress tracking

**What Was Unclear/Missing:**
- Environment setup scripts assume fixed package set before project planning
- Two-phase environment approach (infrastructure first, packages after planning) not documented
- No guidance for WSL-specific issues:
  - Windows PATH conflicts with Linux Python (pyenv shims don't work, use `python3`)
  - Windows paths need `/mnt/d/` prefix in WSL (e.g., `D:\folder` becomes `/mnt/d/folder`)
- Setup scripts don't address application development (only notebooks)

**Suggestion for DSM Improvement:**
- Created BACKLOG-010: Generic Environment Setup Strategy
  - Covers two-phase setup (infrastructure vs project packages)
  - Includes environment tool comparison (venv, poetry, pyenv, conda)
  - Addresses both notebook and application development
- Consider adding WSL/cross-platform notes to Appendix A or DSM_0

---

### Day 1 - Exploration (2026-01-26)

**DSM Sections Referenced:**
- Section 2.2 (Phase 1: Exploration)
- PM Guidelines: Tone and Style (notebook cell requirements)
- Appendix D.2 (NLP Domain)

**What Worked Well:**
- Checkpoint template structure guided comprehensive analysis
- MUST/SHOULD/COULD framework helped prioritize EDA activities
- Output folder convention kept artifacts organized
- Cell-by-cell development protocol ensured iterative validation

**What Was Unclear/Missing:**
- PM Guidelines mention markdown descriptions for cells, but easy to overlook
- No explicit guidance on folder structure (notebooks/, data/, outputs/)
- NLP Appendix D.2 could include common EDA patterns for text data

**Suggestion for DSM Improvement:**
- Add explicit "Notebook Cell Checklist" to PM Guidelines (markdown before code)
- Include recommended folder structure template in Quick Start Guide
- Expand Appendix D.2 with NLP-specific EDA checklist (class balance, text length, special patterns)

---

### Day 2 - Preprocessing & Vectorization (2026-01-26)

**DSM Sections Referenced:**
- Section 2.3 (Phase 2: Feature Engineering)
- Appendix D.2 (NLP Domain - preprocessing steps)
- PM Guidelines: Daily Checkpoint template

**What Worked Well:**
- Day 1 EDA directly informed preprocessing decisions (documented strategy)
- Cell-by-cell protocol caught edge case (empty text) immediately
- Checkpoint template captures variables ready for next phase

**What Was Unclear/Missing:**
- Appendix D.2 could include common TF-IDF parameter ranges for text classification
- No guidance on handling edge cases (empty texts after preprocessing)

**Suggestion for DSM Improvement:**
- Add NLP preprocessing checklist to Appendix D.2 with common parameters
- Include edge case handling patterns (empty strings, encoding issues)

---

### Day 3 - Modeling & Evaluation (2026-01-26)

**DSM Sections Referenced:**
- Section 2.4 (Phase 3: Analysis/Modeling)
- Appendix D.2 (NLP Domain - model recommendations)
- PM Guidelines: Checkpoint template

**What Worked Well:**
- Baseline-then-compare approach (Logistic Regression → Naive Bayes) provided clear progression
- Error analysis framework helped identify model limitations systematically
- Checkpoint template guided comprehensive model documentation (metrics, rationale, limitations)
- Cell-by-cell protocol caught issues early (e.g., confusion matrix interpretation)

**What Was Unclear/Missing:**
- No explicit guidance on model selection criteria for different business contexts (precision vs recall tradeoffs)
- Appendix D.2 could include common evaluation metrics interpretation for NLP classification
- No template for documenting model comparison decisions

**Suggestion for DSM Improvement:**
- Add model comparison checklist to Section 2.4 (metrics to compute, visualization requirements)
- Include domain-specific evaluation guidance in appendices (e.g., when to prioritize recall vs precision)
- Add error analysis template with categories (false positives, false negatives, edge cases)

---

### Day 4 - Model Optimization & Advanced NLP (2026-01-27)

**DSM Sections Referenced:**
- Section 2.4 (Phase 3: Analysis/Modeling)
- Appendix D.2 (NLP Domain)
- PM Guidelines: Checkpoint template

**What Worked Well:**
- Checkpoint-driven documentation captured decision reasoning effectively
- Iterative approach: baseline → optimize → advanced methods told clear story
- Cell-by-cell protocol caught data leakage issue early (split before vectorize)
- Comparing multiple approaches empirically (not assuming "best" model)
- OOV analysis provided concrete evidence for embedding limitations

**What Was Unclear/Missing:**
- No guidance on data leakage prevention checklist for NLP pipelines
- No mention of embedding model cleanup (downloaded ~1.7GB of models)
- Appendix D.2 could include expected performance ranges for common NLP tasks
- No guidance on when to expect embeddings to outperform TF-IDF (task-dependent)

**Suggestion for DSM Improvement:**
- Add "Data Leakage Checklist" to Section 2.3 (fit on train only, transform test)
- Include model/data cleanup guidance for large downloads (gensim-data, HuggingFace cache)
- Add NLP benchmark expectations to Appendix D.2 (when to expect embeddings > TF-IDF)
- Include "surprising negative results are valuable" in communication guidelines

**Workflow Discovery:**
- Claude Code's Edit tool has built-in diff preview (green=add, red=remove)
- This serves as the approval mechanism - no need to describe edits in text first
- Added "File Editing Protocol" section to CLAUDE.md to codify this workflow

---

### Day 5 - Colab Compatibility (2026-01-28)

**DSM Sections Referenced:**
- PM Guidelines: Code Output Standards
- DSM_4.0 Software Engineering (deployment considerations)

**What Worked Well:**
- Iterative testing in Colab revealed multiple compatibility issues
- Each fix was small and isolated (directory creation, package install, auth)
- `exist_ok=True` pattern makes code work in both environments

**What Was Unclear/Missing:**
- No guidance on making notebooks Colab-compatible
- No mention of Kaggle authentication methods (changed from JSON to env var in 2025)
- No checklist for "notebook portability" between environments

**Suggestion for DSM Improvement:**
- Add "Notebook Portability Checklist" to PM Guidelines or Appendix:
  - Directory creation (`os.makedirs(..., exist_ok=True)`)
  - Package installation at notebook start
  - Data download fallbacks (local vs cloud)
  - Runtime selection guidance (CPU vs GPU)
- Include note that external APIs change authentication methods over time
- Add guidance on testing notebooks in multiple environments before delivery

**Language Note:**
- Avoid patriarchal/imperial language in technical writing (e.g., "king", "queen", "master/slave")
- Changed "context became king" → "context changed everything" in README and notebook
- Common word embedding example "king - man + woman = queen" should use alternative examples:
  - "Paris - France + Japan = Tokyo" (geography)
  - "doctor - man + woman = doctor" (profession, shows bias awareness)
  - "good - better = bad - worse" (analogy)

---

### Day 6 - Blog, Publication & Presentation (2026-01-30)

**DSM Sections Referenced:**
- PM Guidelines: Communication deliverables
- DSM_4.0 Software Engineering (deployment, portability)

**What Worked Well:**
- Materials-first approach for blog writing (blog-materials.md before drafting)
- Scoping questions (platform, audience, tone, length) prevented misaligned drafts
- Line-by-line editorial review caught uncited claims, jargon, and factual errors
- Final citation scan found 5 additional missing references and 3 factual issues
- Staggered LinkedIn publication strategy (short post first, article later, comment linking them)

**Issues Found:**

1. **Kaggle authentication in Colab (3 failures, 4 iterations)**
   - **Attempt 1** (previous session): Used `KAGGLE_API_TOKEN` environment variable — Colab's pre-installed `kaggle` package does not recognize this variable, it only reads `kaggle.json`
   - **Attempt 2**: Wrote `kaggle.json` to `~/.config/kaggle/` — wrong path. Kaggle CLI reads from `~/.kaggle/`, not `~/.config/kaggle/`
   - **Attempt 3**: Fixed path to `~/.kaggle/`, but Kaggle changed their API — no longer offers legacy `kaggle.json` downloads. New tokens use `KGAT_` prefix format
   - **Attempt 4**: Upgraded `kaggle` package via `pip install --upgrade kaggle` — broke Colab with `ImportError: cannot import name 'get_access_token_from_env' from 'kagglesdk'` (dependency conflict with Colab's environment)
   - **Final fix**: Bypassed `kaggle` CLI entirely. Used `requests` library with direct Kaggle API call and `KGAT_` token as HTTP bearer token (`Authorization: Bearer KGAT_xxx`)
   - Lesson: External APIs change auth methods over time. CLI wrappers may lag behind or conflict with host environments. Direct HTTP calls with documented API endpoints are the most portable approach

2. **Side-by-side plot scaling (Cell 7)**
   - Two histograms displayed side-by-side with independent y-axes
   - Different scales made visual comparison misleading — proportions appeared distorted
   - Fix: Added `sharey=True` to `plt.subplots()` so both plots share the same y-axis
   - Lesson: Side-by-side comparison plots must share axes to be meaningful

3. **Notebook header outdated**
   - Title still said "Disaster Tweet Classification" (generic)
   - Updated to match blog: "From TF-IDF to Transformers: What Classifying Disaster Tweets Taught Me About How We Got to LLMs"
   - Objective, dataset info, and section list updated to reflect actual content

**Suggestion for DSM Improvement:**
- Add "Visualization Checklist" to PM Guidelines:
  - Side-by-side plots: use `sharey=True` or `sharex=True` for fair comparison
  - Always label axes, include units where applicable
  - Test visualizations at presentation scale (projector/screen), not just notebook
- Add "External API Authentication" note to portability checklist:
  - Always test auth in the target environment (Colab, not local)
  - Prefer direct HTTP API calls over CLI wrappers for portability
  - CLI packages may conflict with host environment dependencies (e.g., Colab's pre-installed packages)
  - External APIs change auth methods — document the method and version tested
  - Bearer token auth via `requests` is more portable than CLI tools
- Add "Blog/Communication Deliverable" as a standard project phase (see dsm-feedback-blog.md for full process)

**Backlog Items Created:**
- BACKLOG-011: LinkedIn Publication Strategy (documented in dsm-feedback-blog.md)

---

### Day 7 - Presentation Feedback & Project Closure (2026-01-30)

**DSM Sections Referenced:**
- Section 2.5 (Communication)
- PM Guidelines: Presentation deliverables
- Appendix D.2 (NLP Domain)

**What Worked Well:**
- Instructor feedback (Marcel De Sutter) provided three concrete methodological insights that DSM should incorporate
- Transcript extraction allowed capturing feedback that might otherwise be lost after the session
- Aligning instructor comments with personal notes ensured complete coverage

**Instructor Feedback — Three Key Topics:**

**1. Embedding Analysis: Cluster Coherence and Visualization**
- When comparing embedding methods (GloVe vs FastText vs Sentence Transformers), use **cluster coherence metrics** and dimensionality reduction visualizations (tSNE, UMAP) to assess quality
- Visualizing clusters reveals whether embeddings separate classes meaningfully, beyond what a single F1 score shows
- Marcel noted that GloVe's architecture (outer product of co-occurrence matrix) is a "proto-attention mechanism" — a single-matrix version of what Transformers do with Q/K/V matrices
- **DSM relevance:** Appendix D.2 should recommend embedding visualization as a standard analysis step when comparing text representations

**2. Data Splitting: Group by Origin, Not Randomly**
- When a dataset contains multiple samples from the same source (e.g., multiple X-rays from one patient, multiple vibration readings from one vehicle), splitting must be done by **source group**, not randomly
- Random splitting risks data leakage: the model memorizes patient-specific or vehicle-specific patterns rather than learning generalizable features
- **Stationarity assumption:** A patient may be healthy at time T1 and sick at T2. If both X-rays are in the training set, the model sees the "answer" implicitly. Similarly, vehicle vibration data changes over time as wear progresses
- **Example cited:** Andrew Ng's lung X-ray project initially achieved high accuracy that collapsed when tested on a different hospital's data — because the model learned hospital-specific imaging artifacts, not pathology
- **DSM relevance:** The existing "Data Leakage Checklist" suggestion (Day 4) should be expanded to include: "If multiple samples share an origin (patient, device, user, session), split by origin ID using `GroupKFold` or equivalent"

**3. Human Performance as Benchmark / Explainability Tradeoff**
- Before modeling, establish a **human performance baseline**: how accurately can a human expert perform the same task? This sets a realistic ceiling for model performance
- **Occam's razor for model selection:** If a simple model (Logistic Regression) achieves 0.764 F1 and a complex model (Sentence Transformers) achieves 0.770, the marginal gain must justify the added complexity
- **Performance vs explainability is an inverse relationship:** As model complexity increases (GLM → tree ensembles → deep learning), explainability decreases
- In **regulated industries** (healthcare, insurance, banking), explainability is not optional. GLMs (Generalized Linear Models) are "100% transparent" — every coefficient is interpretable. EU AI Act introduces legal requirements for model explainability in high-risk applications
- **DSM relevance:** Section 2.4 should include guidance on when explainability requirements constrain model selection. Add a decision framework: "Is this a regulated domain? → Prefer explainable models. Is marginal performance gain worth reduced interpretability?"

**What Was Unclear/Missing:**
- No DSM guidance on incorporating external feedback (instructor, peer review, stakeholder) into the methodology loop
- No framework for deciding when model complexity is justified vs when simpler models suffice (Occam's razor principle)
- No mention of human performance benchmarking as a standard analysis step
- No guidance on embedding visualization and cluster analysis for NLP projects

**Suggestion for DSM Improvement:**
- Add "External Feedback Integration" step to Section 2.5: after presenting results, capture domain expert feedback and document how it would change the analysis
- Add "Human Performance Baseline" to Section 2.4 as a recommended step before modeling
- Add "Model Complexity Justification" principle: document why a more complex model is chosen when a simpler one performs similarly
- Expand Appendix D.2 with embedding analysis checklist: cluster coherence, tSNE/UMAP visualization, silhouette scores
- Add data splitting rules for grouped data: `GroupKFold`, `LeaveOneGroupOut`, split by origin entity

**Backlog Items Created:**
- BACKLOG-012: Human Performance Baseline Protocol
- BACKLOG-013: Model Complexity vs Explainability Decision Framework

---

## Final Closure: Missing Feedback & Project Reflection

This section captures feedback that was not documented during daily entries but emerged from reviewing the full collaboration across 7 days and 3 feedback documents.

### Feedback Not Previously Captured

**1. Multi-Session Context Management**
- This project spanned multiple Claude Code sessions. Context was lost between sessions, requiring re-explanation of decisions and history (e.g., the Kaggle auth saga required re-debugging knowledge that existed in a previous session)
- **DSM gap:** No guidance on managing AI-assisted projects across multiple sessions. Recommend: maintain a running "session state" document (key decisions, current blockers, next steps) that persists between sessions
- **Suggested addition:** Add "Session Handoff" template to DSM Section 6.1 (Session Management)

**2. Structured Debugging with AI Assistants**
- The Kaggle authentication debugging (4 iterations across 2 sessions) showed a pattern: each failed attempt narrowed the solution space, but without systematic tracking, attempts could repeat
- The eventual fix (direct HTTP with bearer token) was only reached after exhausting CLI-based approaches. Documenting each attempt with its failure mode prevented circular debugging
- **DSM gap:** No guidance on debugging workflows when using AI assistants. The assistant generates solutions quickly but may cycle through approaches without learning from previous failures unless the human tracks state
- **Suggested addition:** Add "Debugging Log" template — for each attempt: hypothesis, action taken, result, what it eliminates

**3. AI as Collaborative Writing Partner**
- The blog writing process (documented in dsm-feedback-blog.md) revealed that Claude Code functions differently as a writing partner vs a coding assistant. Writing requires iterative refinement, audience calibration, and citation rigor — skills the AI has but doesn't apply at full strength without explicit prompting
- **DSM gap:** dsm-feedback-blog.md captures this well but it's isolated. The pattern applies to any communication deliverable (presentation slides, documentation, reports)
- **Suggested addition:** Generalize the blog writing protocol into a "Communication Deliverable Protocol" in PM Guidelines

**4. Scope Management During Exploration**
- Days 4-6 expanded significantly beyond the original plan. All COULD items were completed, plus unplanned work (Colab compat, blog post, presentation)
- The MUST/SHOULD/COULD framework helped prioritize within each day, but there was no explicit "scope checkpoint" to decide when to stop expanding
- **DSM gap:** No guidance on when to stop adding scope during exploratory projects. The empirical comparison approach (testing 5 models, 4 vectorization methods) was valuable but could expand indefinitely
- **Suggested addition:** Add "Scope Review" checkpoint to Section 2.4 — after baseline results, explicitly decide whether to pursue advanced methods or move to communication

**5. Cross-Document Consistency**
- This project produced 3 feedback documents, a blog, a methodology doc, checkpoints, and a README. The same facts appear in multiple places (e.g., Kaggle auth method, final F1 scores, project timeline). When one document is updated, others become inconsistent
- **Example:** dsm-feedback-methodology.md Phase 8 still references "API token via `KAGGLE_API_TOKEN` env var" after the method was changed to direct HTTP with bearer token
- **DSM gap:** No guidance on maintaining consistency across project documentation
- **Suggested addition:** Add "Documentation Audit" as a final project step — scan all documents for contradictions after completing the project

**6. Presentation Preparation as a Project Phase**
- The project included a presentation (Marcel De Sutter provided feedback), but presentation preparation was not structured as a DSM phase. The blog process was well-documented (dsm-feedback-blog.md), but presentation prep was ad hoc
- **DSM gap:** No template or checklist for preparing technical presentations
- **Suggested addition:** Add "Presentation Preparation" checklist to PM Guidelines: key results to highlight, demo flow, anticipated questions, backup slides

### Updated Summary (Full Project: Days 0-7)

**Overall DSM Effectiveness:**
- DSM provided strong structure across a 7-day NLP project that expanded from pure development into Colab deployment, blog writing, and presentation
- Checkpoint-driven documentation captured decision reasoning at every stage
- Cell-by-cell notebook protocol caught issues early (data leakage, empty texts, visualization scaling)
- MUST/SHOULD/COULD framework helped prioritize within time constraints and supported scope expansion decisions
- Empirical comparison approach aligned with scientific method and instructor expectations
- The feedback loop (daily entries → summary → instructor feedback → final reflection) produced actionable DSM improvements

**Top 5 Improvements to Propose:**
1. **Data Leakage Checklist** — Section 2.3: "fit on train only, transform test" + grouped splitting for multi-sample data
2. **Human Performance Baseline** — Section 2.4: establish human accuracy before modeling to set realistic targets
3. **Model Complexity vs Explainability Framework** — Section 2.4: decision guide for when simpler models are preferable
4. **Communication Deliverable Protocol** — PM Guidelines: structured process for blog posts, presentations, and reports (scoping → drafting → review → audit)
5. **External API Portability Checklist** — PM Guidelines: prefer direct HTTP over CLI wrappers, test in target environment, document auth method and version

**Backlog Items Created (Full Project):**
- BACKLOG-010: Generic Environment Setup Strategy
- BACKLOG-011: LinkedIn Publication Strategy
- BACKLOG-012: Human Performance Baseline Protocol
- BACKLOG-013: Model Complexity vs Explainability Decision Framework

**Feedback Documents Produced:**
- `docs/dsm-feedback-backlogs.md` — Daily process feedback and improvement suggestions (this file)
- `docs/dsm-feedback-methodology.md` — Final project structure, pipeline, tools, plan vs reality
- `docs/dsm-feedback-blog.md` — Blog writing process, editorial patterns, LinkedIn publication strategy
