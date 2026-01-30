# DSM Feedback: Final Project Methodology

**Project:** Disaster Tweet Classification (Sprint 3)
**Author:** Alberto Diaz Durana
**Date:** January 2026
**Duration:** 7 days (4 development + 1 Colab testing + 1 blog/publication + 1 presentation/closure)

---

## 1. Project Overview

| Item | Planned | Actual |
|------|---------|--------|
| **Objective** | Binary tweet classification | Same |
| **Dataset** | Kaggle NLP Getting Started (train.csv) | Same, 7,613 tweets |
| **Timeline** | 4 days dev + Day 5 presentation | 4 days dev + Day 5 Colab + Day 6 blog + Day 7 presentation |
| **Deliverables** | Notebook + presentation | Notebook + README + blog + 3 DSM feedback docs + presentation |
| **Environments** | Local (VSCode) + Colab | Same, Colab verified on Day 5 |

---

## 2. Technical Pipeline (What Was Actually Built)

### Phase 1: Setup & EDA (Day 1)
- **Data loading:** pandas `read_csv`, Kaggle API fallback for Colab
- **EDA:** Class distribution (57/43 split), text length stats, sample analysis
- **Visualizations:** Bar charts, distribution plots (matplotlib/seaborn)
- **Output:** class_distribution.png, text_length_distribution.png

### Phase 2: Preprocessing (Day 2)
- **Pipeline steps:**
  1. URL removal (`re.sub`)
  2. Lowercasing
  3. Lemmatization (NLTK WordNetLemmatizer)
  4. Stop word removal (NLTK English stopwords)
- **Not used:** Stemming (lemmatization preferred for preserving meaning)
- **Edge case found:** 1 empty text after preprocessing → replaced with "empty"

### Phase 3: Vectorization (Day 2)
- **Method:** TF-IDF (scikit-learn `TfidfVectorizer`)
- **Parameters:** `max_features=5000`, `ngram_range=(1,2)`, `min_df=2`, `max_df=0.95`
- **Result:** ~5,000 sparse features
- **Train/test split:** 80/20, stratified, `random_state=42`

### Phase 4: Baseline Modeling (Day 3)
- **Models tested:** Logistic Regression, Naive Bayes, SVM, Random Forest, XGBoost
- **Winner:** Logistic Regression (F1 = 0.764 after tuning)
- **Evaluation:** accuracy, precision, recall, F1, confusion matrix, cross-validation
- **Hyperparameter tuning:** GridSearchCV on Logistic Regression

### Phase 5: Data Leakage Correction (Day 4)
- **Issue found:** Original pipeline fit TF-IDF on full dataset before split
- **Fix:** Split first → fit TF-IDF on train only → transform test
- **Impact:** Results remained consistent (validated approach)

### Phase 6: Word Embeddings Comparison (Day 4)
- **GloVe-Twitter-200:** Pre-trained on 2B tweets, 200d vectors
  - Document vectors via averaging word vectors
  - F1 = 0.747
- **FastText-300:** Pre-trained, 300d vectors, subword-aware
  - Document vectors via averaging word vectors
  - F1 = 0.755
- **Key insight:** Both lost to TF-IDF. Averaging word vectors loses importance weighting.

### Phase 7: Sentence Transformers (Day 4)
- **Model:** `all-MiniLM-L6-v2` (384d embeddings)
- **Why:** Contextual embeddings — same word gets different vectors in different sentences
- **F1 = 0.770** (best result)
- **Key insight:** Context-awareness is what distinguishes transformer-era NLP

### Phase 8: Colab Compatibility (Day 5)
- **Package installation:** gensim, sentence-transformers, xgboost (not pre-installed in Colab)
- **Directory creation:** `os.makedirs('../outputs/figures', exist_ok=True)`
- **Kaggle auth:** Direct HTTP API call with `KGAT_` bearer token via `requests` library. Bypasses `kaggle` CLI entirely — CLI wrappers conflict with Colab's pre-installed packages and lag behind Kaggle's auth changes (legacy `kaggle.json` deprecated, new `KGAT_` prefix tokens introduced). Endpoint: `https://www.kaggle.com/api/v1/competitions/data/download-all/{competition}`
- **Data paths:** Fallback logic (local path → Colab working directory)
- **Runtime:** T4 GPU recommended for Sentence Transformers encoding

### Phase 9: Blog & Publication (Day 6)
- **Blog preparation:** Materials document (blog-materials.md) with story arc, references, key insights
- **Blog writing:** ~2,700 words, 15 academic citations, collaborative editorial process with Claude Code
- **Publication strategy:** LinkedIn short post → LinkedIn Article → follow-up comment linking them
- **Process documented in:** dsm-feedback-blog.md

### Phase 10: Presentation & Closure (Day 7)
- **Presentation delivery:** Presented to instructor Marcel De Sutter
- **Instructor feedback captured:** Embedding visualization, grouped data splitting, explainability tradeoffs
- **Project closure:** Final reflection across all feedback documents, missing feedback identified
- **Process documented in:** dsm-feedback-backlogs.md (Day 7 entry + Final Closure section)

---

## 3. Libraries & Tools

### Python Libraries

| Library | Version | Purpose |
|---------|---------|---------|
| pandas | 2.3.3 | Data manipulation |
| numpy | 2.2.6 | Numerical operations |
| scikit-learn | — | TF-IDF, models, metrics, GridSearchCV |
| matplotlib | — | Visualizations |
| seaborn | — | Enhanced visualizations |
| nltk | — | Tokenization, stopwords, lemmatization |
| gensim | — | GloVe/FastText loading via `gensim.downloader` |
| sentence-transformers | — | SBERT contextual embeddings |
| xgboost | — | XGBoost classifier |

### NLTK Resources
- `punkt` — Tokenization
- `stopwords` — English stop words
- `wordnet` — Lemmatization

### Pre-trained Models Downloaded
| Model | Size | Source |
|-------|------|--------|
| glove-twitter-200 | ~200MB | gensim-data |
| fasttext-wiki-news-subwords-300 | ~1GB | gensim-data |
| all-MiniLM-L6-v2 | ~80MB | HuggingFace |

### Development Tools
- **IDE:** VSCode with Jupyter kernel (nlp-llms-kernel)
- **Python:** 3.10 (local), 3.12 (Colab)
- **Virtual env:** .venv (local)
- **Version control:** Git + GitHub
- **AI assistant:** Claude Code (Claude Opus 4.5)
- **Final runtime:** Google Colab (T4 GPU)

---

## 4. Final Results

| Rank | Method | F1 Score | Representation |
|------|--------|----------|----------------|
| 1 | Sentence Transformers | **0.770** | Contextual (384d) |
| 2 | TF-IDF + Logistic Regression | 0.764 | Sparse (~5,000 features) |
| 3 | FastText + Logistic Regression | 0.755 | Static (300d) |
| 4 | GloVe-Twitter + Logistic Regression | 0.747 | Static (200d) |

---

## 5. Project Structure (Final)

```
tfidf-to-transformers-with-disaster-tweets/
├── notebooks/
│   └── s03_disaster_tweets.ipynb       # Main notebook (60 cells)
├── data/
│   └── train.csv                       # Kaggle dataset (7,613 tweets)
├── outputs/
│   └── figures/
│       ├── class_distribution.png
│       ├── text_length_distribution.png
│       ├── confusion_matrix_lr.png
│       ├── embeddings_comparison.png
│       └── final_comparison.png
├── docs/
│   ├── plan/
│   │   └── DisasterTweets_Sprint3_Plan.md
│   ├── checkpoints/
│   │   ├── s03_d00_checkpoint.md       # Setup
│   │   ├── s03_d01_checkpoint.md       # EDA
│   │   ├── s03_d02_checkpoint.md       # Preprocessing
│   │   ├── s03_d03_checkpoint.md       # Modeling
│   │   ├── s03_d04_checkpoint.md       # Advanced NLP
│   │   └── s03_d05_checkpoint.md       # Colab compat
│   ├── blog-materials.md               # Blog preparation materials
│   ├── blog-post-draft.md              # Full technical article (~2,700 words)
│   ├── dsm-feedback-backlogs.md        # Process feedback (daily entries)
│   ├── dsm-feedback-methodology.md     # This file
│   └── dsm-feedback-blog.md            # Blog writing process feedback
├── lectures/
│   ├── presentation-qa.md
│   └── (course materials)
└── README.md
```

---

## 6. Plan vs Reality

| Aspect | Planned | Actual | Delta |
|--------|---------|--------|-------|
| **Models** | LR + Naive Bayes | LR, NB, SVM, RF, XGBoost | Extended (COULD item completed) |
| **Vectorization** | TF-IDF only | TF-IDF + GloVe + FastText + SBERT | Extended (COULD item completed) |
| **Evaluation** | accuracy, precision, recall, F1 | Same + cross-validation + GridSearchCV | Extended |
| **Day 4** | Presentation prep | Embeddings + Sentence Transformers | Repurposed for advanced NLP |
| **Day 5** | Presentation delivery | Colab testing + documentation | Adapted |
| **Documentation** | Notebook + checkpoints | + README + blog materials + Q&A | Extended |
| **Libraries** | pandas, numpy, sklearn, nltk | + gensim, sentence-transformers, xgboost | Extended |
| **Data leakage** | Not anticipated | Found and corrected on Day 4 | Unplanned fix |
| **Colab compat** | "Test early" | Full Day 5 required for fixes | Underestimated |

### Key Deviations
1. **Scope expanded:** Completed all COULD items (advanced methods, hyperparameter tuning)
2. **Day 4 repurposed:** Used for embeddings/transformers instead of presentation prep
3. **Day 5 was Colab, not presentation:** Compatibility required more work than planned
4. **Data leakage found:** Not in plan — caught during Day 4 code review
5. **Kaggle auth changed:** Plan assumed `kaggle.json`, reality required 4 iterations ending with direct HTTP bearer token auth (bypassing CLI entirely)
6. **Blog post added:** Full technical article (~2,700 words, 15 citations) not in original plan — emerged as a communication deliverable
7. **Project extended to 7 days:** Original 5-day plan expanded to include blog (Day 6) and presentation/closure (Day 7)

---

## 7. Methodology Observations for DSM

### What This Project Template Should Include
1. **NLP preprocessing checklist:** URL removal, lowercasing, lemmatization vs stemming, stopwords
2. **Data leakage prevention:** Split before vectorization (fit on train only)
3. **Notebook portability checklist:** directories, packages, data download fallbacks, runtime selection
4. **Progressive complexity approach:** Baseline → optimize → compare advanced methods
5. **Embedding comparison framework:** TF-IDF vs static embeddings vs contextual embeddings

### Recommended Standard DSM Feedback Outputs
1. **dsm-feedback-backlogs.md** — Process feedback collected during execution (daily entries + closure)
2. **dsm-feedback-methodology.md** — Final project structure, tools, pipeline, plan vs reality
3. **dsm-feedback-blog.md** — Blog/communication deliverable process feedback (when applicable)
