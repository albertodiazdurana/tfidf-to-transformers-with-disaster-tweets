# Sprint 3 - Day 4 Checkpoint

**Date:** 2026-01-27
**Phase:** Model Optimization & Advanced NLP
**Author:** Alberto Diaz Durana

---

## 1. Starting Point: Day 3 Results

### Baseline Performance

| Model | Accuracy | F1 (Disaster) | Precision | Recall |
|-------|----------|---------------|-----------|--------|
| Logistic Regression | 82.5% | 0.78 | 85% | 72% |
| Naive Bayes | 81.3% | 0.75 | 86% | 67% |

**Selected:** Logistic Regression (better recall - catches more real disasters)

### Error Analysis Revealed Core Problem

- **186 false negatives:** Missed disasters due to metaphorical language, movie references, discussions
- **80 false positives:** Disaster words used casually ("slicker than an oil spill")

**Root cause:** TF-IDF captures word frequency, not meaning. Cannot distinguish:
- "The building is on fire" (literal disaster)
- "That mixtape is fire" (slang for good)

---

## 2. First Decision: Explore Improvements Before Conclusions?

### Question Asked
Before writing conclusions, should we accept 82.5% or explore what's possible?

### Thinking Process
1. Course is about NLP and LLMs - should demonstrate technique evolution
2. Sprint-3.md mentions hyperparameter tuning and cross-validation as optional
3. Lecture includes embedding_demo.ipynb - embeddings are part of curriculum
4. Presentation benefits from showing "what we tried and why"
5. 82.5% leaves room for improvement - worth investigating

### Decision
**Explore improvements** - adds learning value and presentation depth

---

## 3. Second Decision: Which Improvements to Pursue?

### Options Evaluated

#### A. Machine Learning Optimizations

| Technique | Effort | Decision | Reasoning |
|-----------|--------|----------|-----------|
| Remove duplicates | Low | ✅ Yes | 92 duplicates found - clean data matters |
| Cross-validation | Low | ✅ Yes | Single split is unreliable, CV gives confidence intervals |
| More models (SVM, RF, XGBoost) | Low | ✅ Yes | See thinking below |
| Hyperparameter tuning | Medium | ✅ Yes | Mentioned in sprint-3.md as optional |
| Ensemble methods | Medium | ✅ Yes | Combines model strengths |
| Threshold tuning | Low | ✅ Yes | Precision/recall tradeoff matters for use case |

**Key Discussion: Why Not Just Use "Best" Model?**

Initial suggestion: "Use Linear SVM - historically strongest for TF-IDF text classification."

My response: "I would like to not just base the approach on saying that a model has been historically the strongest."

This is correct thinking because:
- Historical performance on other datasets ≠ performance on THIS dataset
- Scientific approach: hypothesis → experiment → conclusion
- Empirical comparison is reproducible and defensible
- Learning value: understand WHY models perform differently

**Decision:** Compare 5 models empirically with cross-validation

#### B. Feature Engineering

| Technique | Decision | Reasoning |
|-----------|----------|-----------|
| Metadata features (length, caps, punctuation) | ❌ Skip | EDA showed high overlap between classes (96 vs 108 chars mean) - likely noise |
| Disaster lexicon | ❌ Skip | Manual curation effort, TF-IDF already captures keyword frequency |
| Named entities | ❌ Skip | Adds complexity, uncertain payoff |
| Character n-grams | ❌ Skip | Better handled by embeddings (FastText) |

**Thinking process:**
- I noted: "Feature engineering doesn't look compelling and might just add noise"
- The core problem is semantic understanding, not feature quantity
- Hand-crafted features are a workaround; embeddings address the root cause
- Course focus is NLP techniques, not generic ML feature engineering

**Decision:** Skip feature engineering, let embeddings capture patterns

#### C. NLP-Specific Improvements

| Technique | Decision | Reasoning |
|-----------|----------|-----------|
| Word Embeddings | ✅ Yes | See detailed analysis below |
| Sentence Transformers | ✅ Yes | Aligns with embedding_demo.ipynb |
| DistilBERT fine-tuning | ⏸️ Defer | Stretch goal if time permits |
| LLM zero-shot | ❌ Skip | Outside course scope |

---

## 4. Third Decision: Which Word Embedding Approach?

### Question Asked
I wanted to understand embeddings in detail: "This project is an opportunity to look into the detail. More code is not a problem."

### Criteria Established
1. **Colab compatibility** - must work in Google Colab
2. **Model size** - reasonable download time
3. **Future relevance** - useful knowledge for future work
4. **LLM understanding** - helps understand how LLMs work

### Options Analyzed

| Approach | LLM Relevance | Colab Friendly | Learning Value |
|----------|---------------|----------------|----------------|
| **spaCy vectors** | Low (abstracted) | Yes | Low - hides mechanics |
| **GloVe** | Medium (count-based) | Yes | Medium - shows embedding lookup |
| **Word2Vec** | High (neural, predictive) | Large files | High - similar to LM objective |
| **FastText** | High (subword = BPE) | ~1GB | High - explains tokenization |
| **GloVe-Twitter** | Medium + domain | ~400MB | High - Twitter-specific |

### Thinking: What Helps Understand LLMs?

LLMs are built on:
1. **Token embeddings** - words/subwords as dense vectors
2. **Contextual representations** - same word → different vector based on context
3. **Prediction objective** - predict next token

**Word2Vec connection to LLMs:**
- Skip-gram: predict context words from target word
- CBOW: predict target word from context words
- This "prediction creates meaning" idea is fundamental to LLMs
- LLMs do next-token prediction - conceptually similar

**FastText connection to LLMs:**
- Uses subword information ("playing" = "play" + "ing")
- LLMs use BPE (Byte Pair Encoding) - same principle
- Handles OOV words - important for real-world text
- Twitter has typos, slang - FastText should handle better

**GloVe-Twitter value:**
- Trained on 2 billion tweets
- Knows Twitter vocabulary, slang, hashtags
- Directly relevant to our disaster tweets dataset
- Shows importance of domain-specific training

### My Decision
"What if we apply both and compare?"

### Why This Approach Works

Comparing GloVe-Twitter vs FastText reveals:

| Aspect | GloVe-Twitter | FastText |
|--------|---------------|----------|
| Word "earthquakeeee" (typo) | ❌ Unknown (zero vector) | ✅ Has vector (subword) |
| Word "covfefe" (Twitter slang) | ✅ Might know (Twitter corpus) | ❌ Probably unknown |
| Subword concept | No | Yes |
| Connection to LLMs | Embedding lookup | Subword tokenization |

**Expected insights:**
- GloVe-Twitter wins on known Twitter vocabulary
- FastText wins on misspellings and novel words
- Both lose on context (same vector for "fire" regardless of meaning)
- This limitation motivates Sentence Transformers

### Decision
**Use both GloVe-Twitter AND FastText** - compare strengths and weaknesses

---

## 5. Final Plan: Complete NLP Progression

### The Story We're Telling

Each step addresses a limitation of the previous:

| Step | Method | What It Captures | Limitation |
|------|--------|------------------|------------|
| 1 | **TF-IDF** | Word frequency | No semantics - "good" and "great" are unrelated |
| 2 | **GloVe-Twitter** | Word meaning (Twitter domain) | No OOV handling, no context |
| 3 | **FastText** | Subword meaning | No context - "fire" always same vector |
| 4 | **Sentence Transformers** | Sentence-level context | Full semantic understanding |

### Why This Progression Matters

For the presentation, we can explain:

1. **TF-IDF baseline:** "We started with TF-IDF because it's simple and interpretable. But it treats 'good' and 'excellent' as completely different features."

2. **Word embeddings:** "We tried pre-trained word embeddings to capture semantic similarity. GloVe-Twitter knows Twitter vocabulary. FastText handles misspellings through subwords - the same technique LLMs use with BPE tokenization."

3. **Embedding comparison:** "GloVe-Twitter performed better on [X], FastText on [Y]. But both give the same vector for 'fire' whether it means disaster or compliment."

4. **Sentence Transformers:** "To capture context, we used transformer-based embeddings. Now 'the building is on fire' and 'that mixtape is fire' get different representations."

5. **Results comparison:** "Here's how each approach performed, and why."

---

## 6. Implementation Plan

### Section 6: Model Optimization (ML Focus)

| Cell | Content |
|------|---------|
| 35 | Section header (done) |
| 36 | Remove duplicates, prepare data |
| 37 | Cross-validation: 5 models comparison |
| 38 | Hyperparameter tuning (GridSearchCV) |
| 39 | Ensemble methods |
| 40 | Threshold tuning |
| 41 | ML optimization summary |

### Section 7: Word Embeddings

| Cell | Content |
|------|---------|
| 42 | Section header + explanation |
| 43 | Load GloVe-Twitter (gensim downloader) |
| 44 | Document embedding function (average word vectors) |
| 45 | Handle OOV words analysis |
| 46 | Train classifier on GloVe embeddings |
| 47 | Load FastText |
| 48 | Train classifier on FastText embeddings |
| 49 | Compare GloVe vs FastText (OOV handling, performance) |
| 50 | Word embeddings summary |

### Section 8: Sentence Transformers

| Cell | Content |
|------|---------|
| 51 | Section header + explanation |
| 52 | Load all-MiniLM-L6-v2 |
| 53 | Generate sentence embeddings |
| 54 | Train classifier |
| 55 | Compare to word embeddings |
| 56 | Sentence transformers summary |

### Section 9: Conclusions

| Cell | Content |
|------|---------|
| 57 | Final comparison table (all methods) |
| 58 | Key insights and learnings |
| 59 | Limitations and future work |
| 60 | References |

---

## 7. Technical Details

### Libraries Required

```python
# ML Optimization
from sklearn.model_selection import cross_val_score, GridSearchCV
from sklearn.svm import LinearSVC
from sklearn.ensemble import RandomForestClassifier, VotingClassifier
from xgboost import XGBClassifier

# Word Embeddings
import gensim.downloader as api
# glove-twitter-200: ~400MB
# fasttext-wiki-news-subwords-300: ~1GB

# Sentence Transformers
from sentence_transformers import SentenceTransformer
# all-MiniLM-L6-v2: ~80MB
```

### Colab Compatibility Notes

| Library | Colab Install | Notes |
|---------|---------------|-------|
| gensim | Pre-installed | Download models on first use |
| xgboost | Pre-installed | Works out of box |
| sentence-transformers | `!pip install sentence-transformers` | Quick install |

### Expected Download Times (Colab)

| Model | Size | Time |
|-------|------|------|
| glove-twitter-200 | ~400MB | ~30-60 sec |
| fasttext-wiki-news-subwords-300 | ~1GB | ~2-3 min |
| all-MiniLM-L6-v2 | ~80MB | ~10-20 sec |

---

## 8. Results: Section 6 - Model Optimization

### Cross-Validation Results (5 models)

| Rank | Model | F1 (CV) | Observation |
|------|-------|---------|-------------|
| 1 | Logistic Regression | 0.731 | Original choice validated |
| 2 | Naive Bayes | 0.725 | Very close second |
| 3 | Random Forest | 0.720 | Complex model, no advantage |
| 4 | Linear SVM | 0.720 | Expected strong, slightly behind |
| 5 | XGBoost | 0.704 | Worst - overfitting sparse data |

**Key insight:** Simpler linear models outperform complex ensemble methods on TF-IDF sparse features.

### Hyperparameter Tuning (GridSearchCV)

Best parameters:
- `tfidf__max_features`: 7000
- `tfidf__ngram_range`: (1, 2)
- `clf__C`: 1.0
- `clf__class_weight`: balanced

**Best CV F1:** 0.751 (up from 0.731 baseline)

### Ensemble & Threshold Results

| Configuration | Accuracy | F1 |
|---------------|----------|-----|
| Tuned LogReg | 79.6% | 0.757 |
| Ensemble (LR+NB+SVM) | 80.1% | 0.753 |
| Threshold=0.53 (optimal) | - | **0.764** |
| Threshold=0.39 (high recall) | - | 0.736 (85% recall) |

**Insight:** Ensemble improves accuracy but hurts F1. Threshold tuning gives best F1.

---

## 9. Results: Section 7 - Word Embeddings

### Performance Comparison

| Method | Accuracy | F1 | Dimensions |
|--------|----------|-----|------------|
| TF-IDF (tuned) | 79.6% | **0.764** | 7000 |
| FastText | 78.4% | 0.755 | 300 |
| GloVe-Twitter | 78.2% | 0.747 | 200 |

**Surprising result:** TF-IDF beats both word embedding approaches!

### OOV Analysis

| Model | OOV Rate | Training Corpus |
|-------|----------|-----------------|
| GloVe-Twitter | 4.67% | 2B tweets |
| FastText | 6.56% | Wikipedia/News |

GloVe-Twitter has better OOV coverage (Twitter corpus), but FastText still slightly outperforms due to 300 dims vs 200.

### Why Word Embeddings Underperformed

1. **Averaging loses information:** Word importance (TF-IDF weights) lost when averaging vectors
2. **Dimensionality:** 7000 sparse features vs 200-300 dense dimensions
3. **Task-specific learning:** TF-IDF learns from OUR data; embeddings are pre-trained on general corpus
4. **Context still missing:** Static embeddings give same "fire" vector regardless of meaning

### Key Learning: Subword Concepts

- **FastText** uses character n-grams → handles typos/compounds
- This is conceptually similar to **BPE tokenization** in LLMs
- Both address OOV by decomposing words into subunits

---

## 10. Results: Section 8 - Sentence Transformers

### Performance

| Method | Accuracy | F1 | Dimensions | Context |
|--------|----------|-----|------------|---------|
| **Sentence Transformers** | **80.3%** | **0.770** | 384 | Yes |
| TF-IDF (tuned) | 79.6% | 0.764 | 7000 | No |
| FastText | 78.4% | 0.755 | 300 | No |
| GloVe-Twitter | 78.2% | 0.747 | 200 | No |

**Sentence Transformers wins!** Contextual embeddings achieve the best F1 score.

### Why Sentence Transformers Won

1. **Context matters:** "Fire" gets different vectors in "building on fire" vs "mixtape is fire"
2. **No averaging:** Entire sentence encoded at once, preserving relationships
3. **Transformer architecture:** Self-attention captures word relationships
4. **Pre-trained on NLI:** Model understands semantic similarity

### The Improvement

- Modest (+0.6% F1 over TF-IDF) but validates the hypothesis
- Contextual embeddings help with metaphorical language
- Same architecture powers GPT, BERT, and modern LLMs

---

## 11. Success Criteria (Final)

| Deliverable | Status |
|-------------|--------|
| Clear reasoning for all decisions | ✅ Complete |
| 5-model cross-validation comparison | ✅ Complete |
| Best model hyperparameter-tuned | ✅ Complete |
| Ensemble attempted | ✅ Complete |
| Data leakage lesson documented | ✅ Complete |
| GloVe-Twitter embeddings tested | ✅ Complete |
| FastText embeddings tested | ✅ Complete |
| Word embedding comparison (OOV analysis) | ✅ Complete |
| Sentence-transformers comparison | ✅ Complete |
| Final results table (all methods) | ✅ Complete |
| Conclusions with insights | ✅ Complete |

**All deliverables complete!**

---

## 12. Alignment with Course Objectives

| Course Goal | How This Project Addresses It |
|-------------|-------------------------------|
| Understand NLP pipeline | Full pipeline: preprocess → vectorize → model → evaluate |
| Text preprocessing | Implemented cleaning, lemmatization, stopwords |
| Text vectorization | TF-IDF + word embeddings + sentence embeddings |
| Model selection | Empirical comparison of 5 models with CV |
| Evaluation metrics | Accuracy, F1, precision, recall, confusion matrix |
| Reason about model behavior | Error analysis, OOV analysis, context limitations |
| Clear communication | Documented decisions, presentation narrative |
| Embeddings (from lecture demo) | GloVe-Twitter, FastText, Sentence Transformers |
| LLM foundations | Subword tokenization concept (FastText ↔ BPE) |

---

## 13. Cleanup Reminder

Delete downloaded embedding models (~1.5GB):
```bash
rm -rf ~/gensim-data/
rm -rf ~/.cache/huggingface/
```
