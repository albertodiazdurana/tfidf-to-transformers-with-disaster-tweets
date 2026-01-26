# Disaster Tweet Classification - Sprint 3 Project Plan

**Project:** NLP Text Classification
**Domain:** Natural Language Processing
**Prepared by:** Alberto Diaz Durana
**Timeline:** 4 days development + Day 5 presentation
**Created:** 2026-01-26

---

## Purpose

**Objective:** Classify tweets as disaster-related (1) or non-disaster (0) using NLP techniques learned in Sprint 1.

**Business Value:** Demonstrate understanding of NLP pipeline from preprocessing to model evaluation. Focus on reasoning and interpretation, not code complexity.

**Deliverables:**
1. Jupyter notebook (developed locally, runnable in Google Colab)
2. Live presentation (reasoning focus, not code walkthrough)

**Development Environment:**
- Local development: VSCode + Jupyter kernel (nlp-llms-kernel)
- Final deliverable: Must run in Google Colab without modification

**Success Criteria:**
- Quantitative: Classification metrics (accuracy, precision, recall, F1-score)
- Qualitative: Clear reasoning, appropriate choices, result interpretation
- Technical: Clean, readable notebook with markdown explanations

---

## Inputs & Dependencies

### Dataset
- **File:** train.csv (~10,000 labeled tweets)
- **Columns:** text (tweet content), target (1=disaster, 0=non-disaster)
- **Source:** Kaggle NLP Getting Started competition
- **Location:** lectures/train.csv

### Reference Materials
| Resource | Purpose |
|----------|---------|
| lectures/sprint1-summary.md | NLP foundations review |
| lectures/sprint-2 | Guided sentiment project patterns |
| lectures/sprint-3.md | Project requirements |
| lectures/masterschool_nlp_llms_01.ipynb | Implementation examples |

### External References
- Kaggle: https://www.kaggle.com/c/nlp-getting-started/overview
- Scikit-learn documentation for models and metrics

---

## Execution Timeline

### Day 1 - Exploration & Data Understanding
**Goal:** Understand the dataset and establish baseline understanding

**Total Time:** 4-6 hours

#### Part 1: Environment & Data Loading (1 hour)
**Objective:** Set up local notebook and load data
**Activities:**
- Create Jupyter notebook locally (VSCode)
- Install/import required libraries
- Load train.csv and inspect structure
- Ensure Colab compatibility
**Deliverables:**
- Working notebook (local + Colab compatible)
- Data loaded and basic info displayed (shape, dtypes, head)

#### Part 2: Exploratory Data Analysis (2-3 hours)
**Objective:** Understand data characteristics and quality
**Activities:**
- Check class distribution (target balance)
- Analyze text length distribution
- Examine sample tweets from each class
- Identify data quality issues (missing values, duplicates)
- Look for patterns (URLs, mentions, hashtags, special characters)
**Deliverables:**
- Class distribution visualization
- Text statistics summary
- Data quality report in markdown
- Initial observations documented

#### Part 3: Business Understanding (1 hour)
**Objective:** Document problem context and approach
**Activities:**
- Write markdown explaining the problem
- Document what makes a tweet "disaster-related"
- Note potential challenges (ambiguity, sarcasm, context)
**Deliverables:**
- Problem statement in notebook
- Initial hypotheses documented

#### End-of-Day 1 Checkpoint
- [ ] Data loaded and inspected?
- [ ] Class distribution understood?
- [ ] Key data characteristics documented?
- [ ] Ready for preprocessing decisions?

**Checkpoint file:** docs/checkpoints/s03_d01_checkpoint.md

---

### Day 2 - Preprocessing & Vectorization
**Goal:** Clean text and transform to numerical features

**Total Time:** 4-6 hours

#### Part 1: Text Preprocessing Pipeline (2-3 hours)
**Objective:** Clean and normalize tweet text
**Activities:**
- Decide on preprocessing steps (document reasoning):
  - Lowercase conversion
  - URL removal
  - Mention (@user) handling
  - Hashtag handling
  - Punctuation/special character removal
  - Stopword removal (evaluate necessity)
  - Stemming vs lemmatization (choose and justify)
- Implement preprocessing function
- Apply to dataset and verify results
**Deliverables:**
- Preprocessing function with comments
- Before/after examples showing transformation
- Markdown explaining each choice

#### Part 2: Text Vectorization (1-2 hours)
**Objective:** Convert text to numerical features
**Activities:**
- Implement TF-IDF vectorization (recommended)
- Consider parameters (max_features, ngram_range)
- Document vectorization choices
**Deliverables:**
- Vectorized feature matrix
- Vocabulary size and feature documentation

#### Part 3: Train/Test Split (30 min)
**Objective:** Prepare data for modeling
**Activities:**
- Split data (e.g., 80/20)
- Ensure stratification for class balance
- Verify split quality
**Deliverables:**
- X_train, X_test, y_train, y_test ready
- Split statistics documented

#### End-of-Day 2 Checkpoint
- [ ] Preprocessing pipeline complete and documented?
- [ ] Vectorization applied?
- [ ] Train/test split done with stratification?
- [ ] Ready for modeling?

**Checkpoint file:** docs/checkpoints/s03_d02_checkpoint.md

---

### Day 3 - Modeling & Evaluation
**Goal:** Train models and evaluate performance

**Total Time:** 4-6 hours

#### Part 1: Baseline Model (1 hour)
**Objective:** Establish performance baseline
**Activities:**
- Train Logistic Regression (simple, interpretable)
- Evaluate with classification_report
- Generate confusion matrix
**Deliverables:**
- Baseline metrics documented
- Confusion matrix visualization

#### Part 2: Model Comparison (2 hours)
**Objective:** Compare different approaches
**Activities:**
- Train alternative model (Naive Bayes recommended)
- Compare metrics between models
- Document trade-offs and observations
**Deliverables:**
- Comparison table of metrics
- Analysis of which model performs better and why

#### Part 3: Model Analysis (1-2 hours)
**Objective:** Understand model behavior
**Activities:**
- Analyze misclassified examples
- Identify patterns in errors
- Document model strengths and weaknesses
- Consider class imbalance effects
**Deliverables:**
- Error analysis with examples
- Model limitations documented

#### Part 4: Optional - Hyperparameter Tuning (if time permits)
**Objective:** Improve model performance
**Activities:**
- Try different C values for Logistic Regression
- Cross-validation for robust estimates
**Deliverables:**
- Tuning results (if attempted)

#### End-of-Day 3 Checkpoint
- [ ] At least one model trained and evaluated?
- [ ] Metrics computed and interpreted?
- [ ] Error analysis complete?
- [ ] Model behavior understood?

**Checkpoint file:** docs/checkpoints/s03_d03_checkpoint.md

---

### Day 4 - Communication & Presentation Prep
**Goal:** Finalize notebook and prepare presentation

**Total Time:** 4-6 hours

#### Part 1: Notebook Finalization (2 hours)
**Objective:** Polish notebook for submission
**Activities:**
- Review all markdown explanations
- Ensure clear flow and structure
- Add summary/conclusions section
- Verify notebook runs end-to-end (local AND Colab)
**Deliverables:**
- Complete, polished notebook

#### Part 2: Presentation Preparation (2-3 hours)
**Objective:** Prepare for live presentation
**Activities:**
- Create presentation outline/slides
- Focus on reasoning, not code:
  - Problem and why it matters
  - Preprocessing approach and rationale
  - Model choice and justification
  - Results interpretation
  - Strengths and limitations
- Practice explaining key decisions
**Deliverables:**
- Presentation slides/outline
- Key talking points prepared

#### Part 3: Q&A Preparation (1 hour)
**Objective:** Anticipate questions
**Activities:**
- Prepare answers for likely questions:
  - "Why did you choose TF-IDF over BoW?"
  - "Why Logistic Regression?"
  - "What do the metrics tell you?"
  - "Where does the model fail?"
**Deliverables:**
- Q&A preparation notes

#### End-of-Day 4 Checkpoint
- [ ] Notebook complete and runs without errors (local + Colab)?
- [ ] Presentation ready?
- [ ] Confident explaining all decisions?

**Checkpoint file:** docs/checkpoints/s03_d04_checkpoint.md

---

### Day 5 - Presentation
**Goal:** Deliver live presentation

**Focus Areas:**
- Problem context and importance
- Approach and reasoning (NOT code walkthrough)
- Results and interpretation
- Honest assessment of limitations

---

## Success Criteria - Priority Framework

### MUST Deliverables (Non-negotiable)
- [ ] Data loaded and explored
- [ ] Text preprocessing implemented with documented reasoning
- [ ] TF-IDF vectorization applied
- [ ] At least one classification model trained (Logistic Regression)
- [ ] Evaluation metrics computed (accuracy, precision, recall, F1)
- [ ] Confusion matrix generated
- [ ] Markdown explanations for all major decisions
- [ ] Notebook runs end-to-end without errors (local + Colab)

### SHOULD Deliverables (Complete if on track)
- [ ] Second model comparison (Naive Bayes)
- [ ] Error analysis with misclassified examples
- [ ] Model strengths/weaknesses documented
- [ ] Presentation slides prepared
- [ ] Q&A preparation complete

### COULD Deliverables (Only if ahead)
- [ ] Hyperparameter tuning with cross-validation
- [ ] Additional preprocessing experiments (stemming vs lemmatization comparison)
- [ ] Advanced method exploration (e.g., word embeddings, transformer-based approach)
- [ ] Feature importance analysis

### Contingency Rules
- If on track after Day 2: Full scope (MUST + SHOULD + COULD)
- If 10-20% behind after Day 2: MUST + SHOULD only
- If >20% behind after Day 2: MUST only, simplify presentation

---

## Required Libraries

Based on project requirements and Sprint 1 foundations:

### Core (Required)
```python
pandas          # Data manipulation
numpy           # Numerical operations
scikit-learn    # ML models, vectorization, metrics
matplotlib      # Visualization
seaborn         # Enhanced visualization
```

### NLP Specific
```python
nltk            # Tokenization, stopwords, stemming, lemmatization
```

### Installation Command (local environment)
```bash
pip install pandas numpy scikit-learn matplotlib seaborn nltk
```

### NLTK Data Downloads (in notebook)
```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
nltk.download('wordnet')
```

### Colab Compatibility Note
All libraries above are pre-installed in Google Colab. NLTK downloads still required.

---

## DSM Feedback Tracking

Per DSM methodology, feedback on DSM effectiveness will be collected after each checkpoint.

**Feedback file:** docs/dsm-feedback.md

**Feedback categories:**
- DSM section referenced
- What worked well
- What was unclear or missing
- Suggestions for improvement

---

## Quality Expectations

Per sprint-3.md requirements:

| Criterion | Expectation |
|-----------|-------------|
| Understanding | Demonstrate grasp of NLP problem |
| Preprocessing | Appropriate and justified choices |
| Modeling | Correct use of classification models |
| Evaluation | Proper interpretation of metrics |
| Reasoning | Clear explanation of decisions |
| Communication | Structured, clear presentation |

**Key Reminder:** Higher complexity does NOT imply higher quality. Simple, well-explained approaches preferred.

---

## Risk Management

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Class imbalance affecting metrics | Medium | Medium | Use stratified split, consider F1 over accuracy |
| Overfitting | Low | Medium | Use train/test split, cross-validation if time |
| Local/Colab compatibility issues | Low | Medium | Test in Colab early, use standard libraries |
| Time overrun on preprocessing | Medium | Medium | Timebox at 3 hours, use simple approach if needed |

---

## References

- DSM Methodology: Section 2.2 (Exploration), Section 2.4 (Analysis)
- NLP Domain: Appendix D.2
- PM Guidelines: Template 1 (Daily Breakdown), Template 7 (Priority Framework)

---

**Next Steps:**
1. Review and approve this plan
2. Install required libraries
3. Create checkpoint and feedback file templates
4. Begin Day 1 execution
