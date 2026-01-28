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

## Summary (End of Project)

**Overall DSM Effectiveness:**
- DSM provided strong structure for a 4-day NLP project
- Checkpoint-driven documentation captured decision reasoning effectively
- Cell-by-cell notebook protocol caught issues early (data leakage, edge cases)
- MUST/SHOULD/COULD framework helped prioritize within time constraints
- Empirical comparison approach (vs "use best model") aligned with scientific method

**Top 3 Improvements to Propose:**
1. **Data Leakage Checklist for NLP** - Add to Section 2.3: "fit on train only, transform test" pattern
2. **Model/Data Cleanup Guidance** - Include cache cleanup instructions (~1.7GB for embeddings models)
3. **NLP Benchmark Expectations** - Add to Appendix D.2: when to expect embeddings > TF-IDF (task-dependent)

**Backlog Items Created:**
- BACKLOG-010: Generic Environment Setup Strategy
