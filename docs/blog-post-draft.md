# From TF-IDF to Transformers: What Classifying Disaster Tweets Taught Me About How We Got to LLMs

*By Alberto Diaz Durana | January 2026*

---

I expected word embeddings to outperform TF-IDF. GloVe was trained on 2 billion tweets. FastText uses the same subword technique found in GPT. Surely these would beat a simple word frequency model on a tweet classification task?

They didn't. And understanding *why* taught me more about modern NLP than any lecture could.

This article walks through a progressive NLP experiment I built during my MasterSchool Data Science program. The task was straightforward: classify tweets as disaster-related or not. But the my personal goal was to **showcase how we got to LLMs** by implementing each generation of text representation on the same problem and comparing results empirically.

---

## The Problem: Same Words, Different Meanings

The dataset comes from Kaggle's [NLP Getting Started](https://www.kaggle.com/c/nlp-getting-started) competition: 7,613 labeled tweets, each classified as disaster (1) or non-disaster (0).

The challenge becomes clear when you look at examples:

- **"The building is on fire"** -- Disaster
- **"That mixtape is fire"** -- Not a disaster

Both contain "fire." Both are short, informal tweets. A model that just counts words will treat them the same. To classify correctly, a model needs to understand *context* -- and that's exactly the problem that drove NLP from TF-IDF to transformers.

The class distribution was slightly imbalanced: 57% non-disaster, 43% disaster. Enough imbalance to make F1 score the right metric instead of accuracy.

---

## Phase 1: TF-IDF -- Teaching Machines to Read

**TF-IDF (Term Frequency-Inverse Document Frequency)** dates back to the 1970s, when Karen Spärck Jones introduced the concept of inverse document frequency as a measure of term specificity (Spärck Jones, 1972). The idea: a word is important if it appears frequently in a document but rarely across the corpus.

The word "earthquake" appearing in a tweet is meaningful because most tweets don't contain it. The word "the" is not meaningful because it appears everywhere.

### Preprocessing

Before vectorizing, I applied a text normalization pipeline as described in Jurafsky & Martin's *Speech and Language Processing* (2024, Ch. 2):

1. **URL removal** -- Links don't help classify disaster vs. non-disaster
2. **Lowercasing** -- "Fire" and "fire" should be treated identically
3. **Lemmatization** -- Reduce words to base form ("running" becomes "run")
4. **Stop word removal** -- Remove common words like "the", "is", "at"

I chose lemmatization over stemming because it preserves word meaning. An aggressive stemmer like Lancaster would reduce "better" to "bet," which loses semantic information. Lemmatization correctly maps it to "good."

### Vectorization

I configured TF-IDF with bigrams (`ngram_range=(1,2)`) to capture two-word phrases like "earthquake hits" or "building collapsed." This produced roughly 5,000 sparse features after setting `max_features=5000` (keep only the top 5,000 most informative terms), `min_df=2` (ignore terms appearing in fewer than 2 documents -- likely typos or noise), and `max_df=0.95` (ignore terms appearing in over 95% of documents -- too common to be discriminative).

**Important detail:** I split the data *before* fitting TF-IDF. The vectorizer learns vocabulary only from training data, then transforms the test set. Fitting on the full dataset before splitting creates data leakage -- the model would "know" test set vocabulary during training. I caught this during a code review on Day 4 and corrected it.

---

## Phase 2: Finding the Right Model

With TF-IDF features ready, I tested five classifiers using 5-fold cross-validation:

| Model | Cross-Val F1 |
|-------|-------------|
| **Logistic Regression** | **Best** |
| Naive Bayes | Close second |
| SVM | Competitive |
| Random Forest | Lower |
| XGBoost | Lower |

Naive Bayes is worth noting: it's a go-to baseline for text classification (McCallum & Nigam, 1998) because it works well with word counts and trains very fast. It came close to Logistic Regression here.

**The finding:** Linear models (Logistic Regression, Naive Bayes, SVM) outperformed tree-based ensemble methods (Random Forest, XGBoost) on this task.

**Why?** TF-IDF produces high-dimensional, sparse data -- thousands of features, most of which are zero for any given tweet. Linear models handle this well because they can learn a weight for each feature independently. Tree-based models, which partition the feature space through binary splits, struggle with high-dimensional sparse inputs and tend to overfit.

I used GridSearchCV for hyperparameter tuning -- an exhaustive search over a grid of parameter values. This made sense here because Logistic Regression has few hyperparameters (`C`, `penalty`), so the search space is small. For models with many parameters, randomized or Bayesian search would be more practical. After tuning, Logistic Regression achieved **F1 = 0.764**.

A confusion matrix revealed the error patterns: the model struggled with metaphorical language ("drowning in work"), disaster words used casually ("slicker than an oil spill"), and hypothetical scenarios ("if firefighters acted like cops").

The common thread? Context. The same word means different things depending on surrounding words. TF-IDF treats each word independently -- it can't solve this.

---

## Phase 3: Word Embeddings -- Words as Vectors with Meaning

Word embeddings represent each word as a list of numbers (a vector) where words with similar meanings end up with similar numbers. The idea was popularized by Word2Vec (Mikolov et al., 2013), which showed that words could be represented as vectors that capture semantic relationships. Unlike TF-IDF, where each word is an independent feature, embeddings place semantically similar words close together: "earthquake" near "tremor," "flood" near "deluge."

There are many word embedding methods available -- Word2Vec, GloVe, FastText, ELMo, among others. I chose two that gave the strongest test for my hypothesis while connecting to the LLM story:

- **GloVe-Twitter** -- trained on 2 billion tweets, giving it the best possible domain match with our dataset. If any static embedding should excel on tweets, it's this one.
- **FastText** -- its subword approach (breaking words into character n-grams) connects directly to subword tokenization used in modern LLMs -- BPE in GPT, WordPiece in BERT -- tying it to the "path to LLMs" story.

I didn't use Word2Vec directly because GloVe builds on the same concept but uses word co-occurrence across the entire corpus rather than just nearby words, making it a natural next step. ELMo (Peters et al., 2018) was skipped because, while it does produce context-dependent vectors, it uses LSTMs -- not the transformer architecture that actually powers GPT and BERT. Since the goal was to trace the path *to transformers specifically*, Sentence Transformers in Phase 4 was the more relevant choice.

### GloVe-Twitter (200 dimensions)

GloVe (Global Vectors for Word Representation) learns embeddings from word co-occurrence statistics -- this means it counts how often words appear near each other across the entire corpus and uses those patterns to build vectors. Words that frequently co-occur with similar neighbors get similar vectors. The Twitter variant was trained on 2 billion tweets -- exactly the domain of our dataset. Each word becomes a 200-dimensional vector.

To represent an entire tweet, I averaged all its word vectors into a single document vector.

**Result: F1 = 0.747**

### FastText (300 dimensions)

FastText represents words as bags of character n-grams -- short sequences of consecutive characters. For example, the word "earthquake" isn't just one token -- it's broken into overlapping pieces like "ear", "art", "rth", "thq", etc. This means FastText can generate vectors for words it has never seen before (out-of-vocabulary words) by combining known subword pieces.

This subword approach is conceptually related to Byte Pair Encoding (BPE) (Sennrich et al., 2016), the tokenization method used in GPT and other large language models.

**Result: F1 = 0.755**

### The Surprise

| Method | F1 Score |
|--------|----------|
| TF-IDF (tuned) | **0.764** |
| FastText | 0.755 |
| GloVe-Twitter | 0.747 |

**Both embedding approaches lost to TF-IDF.**

GloVe was trained on 2 billion tweets. FastText captures subword information. How did a simple word frequency model beat them?

---

## Why Did Embeddings Lose?

Understanding this failure is the most valuable part of the experiment.

### 1. Averaging Destroys Information

To classify a tweet, I needed a single vector per document. The standard approach for word embeddings is to average all word vectors. But averaging treats every word equally:

```
Tweet: "deadly earthquake kills hundreds"

TF-IDF: "earthquake" gets high weight, "the" gets near zero
Embeddings: Average of all vectors -- word importance is lost
```

TF-IDF inherently weights words by importance. Averaging embeddings does not.

### 2. Dimensionality and Discrimination

TF-IDF created ~5,000 sparse features -- each feature is simply a number that tells the model something about the input, in this case the importance of a specific word or bigram. So the classifier gets 5,000 individual signals and can learn that "earthquake" strongly predicts disaster while "lol" predicts non-disaster.

Embeddings compress this into 200-300 dense dimensions. These dimensions capture semantic relationships (words with similar meanings are close), but they're not optimized for *discriminating* between disaster and non-disaster.

**Embeddings optimize for similarity. Classification needs discrimination.** These are fundamentally different objectives.

### 3. The Context Problem Remains

This is the most important reason. Static word embeddings give the same vector to "fire" regardless of context:

- "The building is on fire" --> fire = [0.2, -0.5, 0.8, ...]
- "That mixtape is fire" --> fire = [0.2, -0.5, 0.8, ...]

Same vector. Same meaning, as far as the model is concerned. The metaphor problem that TF-IDF couldn't solve? Word embeddings can't solve it either.

Both TF-IDF and static embeddings treat words as having fixed meanings. The only difference is how they represent those fixed meanings (sparse counts vs. dense vectors). Neither can handle polysemy -- the fact that words have multiple meanings depending on context.

---

## Phase 4: Sentence Transformers -- Context Changed Everything

Sentence Transformers (SBERT) are based on the transformer architecture (Vaswani et al., 2017) -- the same architecture behind BERT (Devlin et al., 2019) and GPT (Radford et al., 2018). The fundamental difference from static embeddings: **the same word produces different vectors depending on its context.**

The Sentence Transformers library (Reimers & Gurevych, 2019) offers several pre-trained models with different tradeoffs:

| Model | Layers | Dimensions | Speed | Quality |
|-------|--------|-----------|-------|---------|
| `all-mpnet-base-v2` | 12 | 768 | Slower | Highest |
| `all-MiniLM-L6-v2` | 6 | 384 | 5x faster | Slightly lower |
| `paraphrase-MiniLM-L6-v2` | 6 | 384 | 5x faster | Paraphrase-focused |

I chose `all-MiniLM-L6-v2` because it offers the best balance of speed and quality. It's a distilled model -- trained to replicate the output of larger models using only 6 layers instead of 12 (Wang et al., 2020). This makes it 5x faster than `all-mpnet-base-v2` while retaining most of its accuracy. The `paraphrase-` variant was designed for a narrower task (paraphrase detection), whereas the `all-` prefix means it was trained on over 1 billion sentence pairs across diverse tasks, making it better suited for general-purpose classification.

Instead of averaging word vectors, the model processes the entire sentence and outputs a single 384-dimensional vector that captures the full contextual meaning.

```
"The building is on fire" --> [0.3, -0.1, 0.7, ...]
"That mixtape is fire"    --> [-0.2, 0.4, 0.1, ...]
```

Different contexts, different vectors. The model understands that "fire" means different things in these two sentences.

**Result: F1 = 0.770** -- The best score across all methods.

---

## The Full Picture

| Rank | Method | F1 | Representation |
|------|--------|-----|----------------|
| 1 | **Sentence Transformers** | **0.770** | Contextual (384d) |
| 2 | TF-IDF + Logistic Regression | 0.764 | Sparse (~5,000 features) |
| 3 | FastText + Logistic Regression | 0.755 | Static (300d) |
| 4 | GloVe-Twitter + Logistic Regression | 0.747 | Static (200d) |

The margin between Sentence Transformers and TF-IDF was modest (+0.6% F1). For many practical text classification tasks, TF-IDF remains a competitive baseline. The real value of contextual embeddings becomes more apparent with larger datasets or tasks requiring deeper semantic understanding.

But the ranking tells the deeper story: **static representations (TF-IDF, GloVe, FastText) all share the same limitation. Context-aware representations solve it.**

---

## The Evolution That Led to LLMs

Building each approach on the same problem traced the progression that produced modern large language models:

```
TF-IDF (1972)
    "Words are independent counts"
        |
Word2Vec / GloVe (2013-2014)
    "Words are dense vectors with semantic meaning"
        |
FastText (2016)
    "Subwords handle unknown words"
    (same idea as BPE tokenization in GPT)
        |
Transformers / BERT (2018+)
    "Context determines meaning"
```

Each step solved a real limitation of the previous one:

- **TF-IDF** couldn't capture that "good" and "excellent" mean similar things
- **Word embeddings** couldn't handle words with multiple meanings
- **Transformers** solved both by making representations context-dependent

The subword approach in FastText is particularly interesting because it connects directly to how LLMs tokenize text. GPT doesn't process whole words -- it breaks them into subword tokens using BPE, conceptually similar to FastText's character n-grams. Understanding FastText makes the tokenization layer of LLMs less mysterious.

---

## Key Takeaways

**1. Always test empirically.** I read that SVM is "historically the strongest" for text classification (Joachims, 1998). On my data, Logistic Regression won. The best model depends on your specific dataset, not on general rules of thumb.

**2. Simple models are strong baselines.** TF-IDF + Logistic Regression took minutes to train, required no GPU, downloaded no pre-trained models, and nearly matched a transformer-based approach. Start simple. You might not need more.

**3. Embeddings capture similarity, but classification needs discrimination.** Word embeddings are excellent for tasks like finding related words or measuring document similarity. But classification needs features that separate classes, not features that group semantics.

**4. Understand *why* your model fails.** The error analysis revealed that both TF-IDF and static embeddings fail on the same examples -- metaphors, sarcasm, figurative language. Understanding the shared failure mode pointed directly to the solution: contextual representations.

**5. The journey matters more than the destination.** I could have started with a fine-tuned BERT model and likely achieved a higher F1 score. But I wouldn't have understood *why* it works. Building each generation of text representation gave me intuition that no tutorial could provide.

---

## Try It Yourself

The full notebook is available on GitHub: [tfidf-to-transformers-with-disaster-tweets](https://github.com/bertodiaz/tfidf-to-transformers-with-disaster-tweets)

It runs end-to-end in Google Colab (T4 GPU recommended). The dataset auto-downloads from Kaggle -- you'll need an API token from [Kaggle Settings](https://www.kaggle.com/settings) and to [accept the competition rules](https://www.kaggle.com/c/nlp-getting-started/rules).

What surprising results have you found in your ML projects? Sometimes the "wrong" answer teaches us the most.

---

**Tags:** #NLP #MachineLearning #DataScience #TextClassification #Transformers #DeepLearning

---

## References

- Jurafsky, D. & Martin, J. H. (2024). *Speech and Language Processing* (3rd ed. draft). Ch. 2: Regular Expressions, Text Normalization, Edit Distance. Available at https://web.stanford.edu/~jurafsky/slp3/
- Spärck Jones, K. (1972). *A Statistical Interpretation of Term Specificity and Its Application in Retrieval.* Journal of Documentation, 28(1), 11-21.
- Joachims, T. (1998). *Text Categorization with Support Vector Machines: Learning with Many Relevant Features.* ECML 1998. Lecture Notes in Computer Science, vol 1398. Springer.
- McCallum, A. & Nigam, K. (1998). *A Comparison of Event Models for Naive Bayes Text Classification.* AAAI-98 Workshop on Learning for Text Categorization, 41-48.
- Mikolov, T., Chen, K., Corrado, G. & Dean, J. (2013). *Efficient Estimation of Word Representations in Vector Space.* ICLR Workshop. arXiv:1301.3781.
- Pennington, J., Socher, R., & Manning, C. D. (2014). *GloVe: Global Vectors for Word Representation*
- Sennrich, R., Haddow, B. & Birch, A. (2016). *Neural Machine Translation of Rare Words with Subword Units.* ACL 2016. arXiv:1508.07909.
- Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., Kaiser, L. & Polosukhin, I. (2017). *Attention Is All You Need.* NeurIPS 2017. arXiv:1706.03762.
- Peters, M. E., Neumann, M., Iyyer, M., Gardner, M., Clark, C., Lee, K. & Zettlemoyer, L. (2018). *Deep contextualized word representations.* NAACL 2018. arXiv:1802.05365.
- Radford, A., Narasimhan, K., Salimans, T. & Sutskever, I. (2018). *Improving Language Understanding by Generative Pre-Training.* OpenAI.
- Devlin, J., Chang, M., Lee, K. & Toutanova, K. (2019). *BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding.* NAACL 2019. arXiv:1810.04805.
- Bojanowski, P., Grave, E., Joulin, A., & Mikolov, T. (2017). *Enriching Word Vectors with Subword Information* (FastText)
- Reimers, N., & Gurevych, I. (2019). *Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks*
- Wang, W., Wei, F., Dong, L., Bao, H., Yang, N. & Zhou, M. (2020). *MiniLM: Deep Self-Attention Distillation for Task-Agnostic Compression of Pre-Trained Transformers.* NeurIPS 2020. arXiv:2002.10957.
- Sentence Transformers Pretrained Models: https://www.sbert.net/docs/sentence_transformer/pretrained_models.html
