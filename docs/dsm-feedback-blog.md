# DSM Feedback: Blog Post Creation Process

**Project:** Disaster Tweet Classification (Sprint 3)
**Author:** Alberto Diaz Durana
**Date:** January 2026
**Document:** How the blog post was written using Claude Code as a collaborative writing partner

---

## 1. Overview

After completing the NLP project (Days 1-5), a blog post was written to share the findings publicly on LinkedIn. The blog was created collaboratively with Claude Code (Claude Opus 4.5) over the course of two sessions. This document captures the full process, interaction patterns, and feedback for DSM methodology.

**Final output:** `docs/blog-post-draft.md` — ~2,700 words, 15 citations, ready for LinkedIn Article publication.

---

## 2. Pre-Writing Phase: Materials Collection

Before writing the blog, a preparation document was created: `docs/blog-materials.md`. This document collected:

- 5 working title options
- The hook (opening paragraph)
- Story arc (6-part structure)
- Key insights (5 numbered takeaways)
- Technical details (code snippets, NLP progression diagram)
- Error analysis examples (false positives and false negatives)
- Figures available from the project
- References (papers and concepts to link)
- A LinkedIn short post draft
- Call to action ideas

**DSM Feedback:** This preparation step was valuable. Having all raw materials in one document before writing made the drafting process faster and more organized. DSM should recommend a "blog materials" or "communication prep" document as a standard deliverable when a project includes public communication.

---

## 3. The Writing Process: Step by Step

### Step 1: Scoping Questions

Before generating any content, Claude asked four scoping questions:

| Question | User's Answer |
|----------|---------------|
| Where will this be published? | LinkedIn |
| Who is the audience? | Mix of all (technical + non-technical) |
| What tone? | Technical tutorial |
| What length? | Long (~2,500 words) |

**DSM Feedback:** Asking scoping questions before writing prevented misaligned drafts. This pattern (clarify → draft → refine) should be standard for any communication deliverable in DSM.

### Step 2: First Draft Generation

Claude generated a complete first draft based on:
- The `blog-materials.md` preparation document
- The project notebook and results
- The scoping answers above

The draft followed the story arc from blog-materials.md:
1. Hook (embeddings losing to TF-IDF)
2. The problem (same words, different meanings)
3. TF-IDF baseline
4. Model selection and optimization
5. Word embeddings comparison
6. Why embeddings lost (3 reasons)
7. Sentence Transformers solution
8. The evolution that led to LLMs
9. Key takeaways
10. Try it yourself (call to action)

### Step 3: Line-by-Line Review (The Core Process)

This was the most time-intensive and valuable phase. The user reviewed the blog **line by line**, requesting changes that fell into distinct categories:

#### Category A: Citation Requests

The user consistently asked for academic citations to back up claims. Every time the blog made a statement based on outside knowledge, the user asked for a reference.

| Claim in Blog | Citation Added |
|---------------|---------------|
| TF-IDF as a text representation method | Spärck Jones (1972) |
| "standard NLP pipeline" for preprocessing | Jurafsky & Martin (2024, Ch. 2) |
| Naive Bayes as "a go-to baseline for text classification" | McCallum & Nigam (1998) |
| Word embeddings concept | Mikolov et al. (2013) — Word2Vec |
| SVM "historically the strongest" for text classification | Joachims (1998) |
| BPE tokenization | Sennrich et al. (2016) |
| Transformer architecture | Vaswani et al. (2017) |
| ELMo uses LSTMs | Peters et al. (2018) |
| GPT | Radford et al. (2018) |
| BERT | Devlin et al. (2019) |

**Pattern observed:** The user treated the blog as an academic-quality document. No claim from outside knowledge was allowed without a citation. This resulted in 15 references in the final blog — more than many published tutorials.

**DSM Feedback:** When generating communication deliverables (blog posts, presentations), DSM should include a "citation audit" step. The AI assistant should proactively flag claims that originate from outside knowledge and suggest citations, rather than waiting for the user to catch them.

#### Category B: Language Simplification

The user repeatedly asked for simpler language when technical jargon was used without explanation. Examples:

| Original | User Feedback | Revised |
|----------|---------------|---------|
| "dense vectors in a continuous space" | "simply put..." | "a list of numbers (a vector) where words with similar meanings end up with similar numbers" |
| "conditional independence assumption aligns naturally with bag-of-words representations" | "phrase it in simpler words" | "it works well with word counts and trains very fast" |
| "GloVe improves on the same idea (global co-occurrence vs. local context windows)" | "too cryptic, in simple words" | "GloVe builds on the same concept but uses word co-occurrence across the entire corpus rather than just nearby words" |
| "ELMo was skipped because it already produces different vectors depending on context" | "not compelling" | "ELMo uses LSTMs -- not the transformer architecture that actually powers GPT and BERT" |

**Pattern observed:** The user's standard was: if a reader has to pause and look something up, the explanation needs rewriting. Technical accuracy was not sacrificed — the language was made accessible while remaining precise.

**DSM Feedback:** DSM communication guidelines should include a "jargon check": after drafting, scan for any technical term used without an inline explanation. The target audience determines what counts as jargon.

#### Category C: Justification of Choices

The user asked "why?" for every technical decision presented in the blog:

| Decision | User Asked | Explanation Added |
|----------|-----------|-------------------|
| GridSearchCV for tuning | "why did we choose GridSearchCV?" | Logistic Regression has few hyperparameters, so exhaustive search is practical |
| GloVe and FastText among many options | "why did we choose glove and fasttext among many other?" | GloVe: best domain match (Twitter). FastText: BPE connection to LLM story |
| Alternatives not considered | "which could have been other possible approaches and why weren't they considered" | Word2Vec skipped (GloVe is a natural extension), ELMo skipped (uses LSTMs, not transformers) |
| all-MiniLM-L6-v2 model | "explain other options and why I didn't select them" | Added comparison table (mpnet vs MiniLM vs paraphrase), speed/quality tradeoffs, distillation explanation |

**Pattern observed:** The user wanted the blog to answer not just "what I did" but "why I did it and what I didn't do and why." This defensive writing style anticipates reader questions.

**DSM Feedback:** DSM communication guidelines should include a "decision justification" principle: every technical choice presented in a blog or presentation should include (1) what was chosen, (2) what alternatives existed, and (3) why the choice was made.

#### Category D: Factual Accuracy

The user asked for a full citation scan at the end. Claude identified and fixed:

| Issue | Type | Fix |
|-------|------|-----|
| "BPE tokenization used in GPT and BERT" | Factual error — BERT uses WordPiece, not BPE | Corrected to "BPE in GPT, WordPiece in BERT" |
| "TF-IDF (2000s)" in evolution diagram | Inconsistency — line 32 says 1970s | Changed to "TF-IDF (1972)" |
| "Stemming would reduce 'better' to 'bet'" | Imprecise — only Lancaster stemmer does this | Added "An aggressive stemmer like Lancaster" |

**DSM Feedback:** A systematic "accuracy scan" at the end of blog writing caught errors that line-by-line editing missed. DSM should recommend this as a final step: scan the entire document for (1) missing citations, (2) factual errors, (3) internal inconsistencies.

#### Category E: Inclusive Language

Earlier in the project, the user established a principle: no patriarchal or imperial language. This carried into the blog:

- "Context became king" → "Context changed everything"
- No "king/queen" analogies in word embedding examples

**DSM Feedback:** Language sensitivity preferences should be documented in CLAUDE.md or project settings so they persist across sessions and apply to all deliverables automatically.

---

## 4. Interaction Summary: By the Numbers

| Metric | Count |
|--------|-------|
| Scoping questions before writing | 4 |
| Total editorial exchanges | ~25 |
| Citations requested by user | 10 |
| Citations found in final scan | 5 additional |
| Language simplifications | 4+ |
| Factual corrections | 3 |
| Rejected edits (user asked for redo) | 3 |
| Final references in blog | 15 |

---

## 5. What Worked Well

1. **Materials-first approach.** Having `blog-materials.md` ready before writing meant the first draft was structurally solid. Most editing was about quality, not structure.

2. **Line-by-line review.** The user's meticulous approach caught issues that a quick skim would miss. Every paragraph was scrutinized for claims, clarity, and completeness.

3. **Iterative refinement.** When Claude's first edit wasn't right (e.g., Naive Bayes phrasing, ELMo explanation), the user rejected it and gave specific feedback. This back-and-forth produced better results than a single-pass revision.

4. **Final citation scan.** Asking Claude to systematically scan for uncited claims caught 5 missing citations and 3 factual issues that line-by-line editing had missed.

5. **Scoping questions.** Asking about platform, audience, tone, and length before writing prevented a misaligned first draft.

---

## 6. What Could Be Improved

1. **Proactive citation flagging.** Claude should have flagged uncited claims during the initial draft, not waited for the user to ask line by line. The final scan proved this was possible — it should happen during drafting.

2. **Factual verification during writing.** The BERT/BPE error (BERT uses WordPiece, not BPE) was written by Claude in the first draft and survived until the final scan. The AI should verify factual claims as it writes them.

3. **Language calibration.** Claude's default technical writing level was too dense for the stated audience ("mix of all"). Several rounds of simplification could have been avoided if the first draft had been calibrated to the audience from the start.

4. **Decision justification by default.** When presenting technical choices (model selection, embedding method, etc.), Claude should automatically include alternatives and reasoning, not wait for the user to ask "why?"

---

## 7. Recommendations for DSM

### New Standard: Blog/Communication Deliverable Process

When a project includes a public communication deliverable (blog post, article, presentation), DSM should recommend:

1. **Preparation phase:** Create a materials document (raw insights, results, story arc, references)
2. **Scoping phase:** Clarify platform, audience, tone, length before drafting
3. **Drafting phase:** Generate first draft from materials
4. **Review phase:** Line-by-line review with focus on:
   - Citation completeness (every outside-knowledge claim needs a reference)
   - Language accessibility (no unexplained jargon for the target audience)
   - Decision justification (what, why, and what alternatives were rejected)
   - Inclusive language compliance
5. **Audit phase:** Systematic scan for:
   - Missing citations
   - Factual accuracy
   - Internal consistency
6. **Publication phase:** Format for target platform, prepare short-form version if needed

### New CLAUDE.md Directive

Add to CLAUDE.md for projects that include blog writing:

```
## Blog Writing Protocol
- Flag claims from outside knowledge that need citations during drafting
- Calibrate language complexity to stated audience
- Include decision justification by default (what was chosen, what wasn't, why)
- Run citation and accuracy audit before finalizing
```

---

## 8. BACKLOG-011: LinkedIn Publication Strategy

**Context:** After writing the blog, we discovered that publishing requires a strategy — not just pasting content. The sequence matters: a short post builds audience, the article delivers depth, and a follow-up comment connects the two. This backlog captures the full publication workflow for DSM.

### 8.1 Deliverables and Format

A project blog produces three distinct pieces of content:

| Deliverable | Format | Length | Purpose |
|-------------|--------|--------|---------|
| **Short post** | LinkedIn post (plain text) | 150-300 words | Hook, key results, build curiosity |
| **Full article** | LinkedIn Article (rich text) | 1,500-3,000 words | Complete technical narrative with citations |
| **Follow-up comment** | Comment on the short post | 2-3 sentences | Link the short post to the full article |

### 8.2 Publication Sequence and Timing

The short post should come **first**, before the article exists. This is what happened in this project — the short post was published on Day 5, and the full article was written afterwards. This sequence works well:

| Step | When | Action |
|------|------|--------|
| 1 | Project completion (Day 5) | Publish short post with key results and a promise of the full article |
| 2 | After blog is finalized | Publish LinkedIn Article with full content |
| 3 | Immediately after step 2 | Comment on the original short post with the article link |

**Why this order works:**
- The short post captures momentum while the project is fresh
- It tests whether the topic generates engagement before investing in the full article
- The follow-up comment notifies everyone who engaged with the original post
- LinkedIn's algorithm favors new posts over comments, so the short post gets organic reach while the comment delivers on the promise

**If the article is ready at the same time as the short post**, publish the article first, then publish the short post with the link embedded. But in practice, the article takes longer to write, review, and cite — so the staggered approach is more realistic.

### 8.3 LinkedIn Article Formatting

LinkedIn Articles do **not** render markdown. When converting from a markdown draft:

| Markdown Element | LinkedIn Article Support | Workaround |
|------------------|--------------------------|------------|
| `#` Headings | Yes (use toolbar) | Apply heading styles manually |
| `**Bold**` / `*Italic*` | Yes (use toolbar) | Apply via toolbar or paste from rendered preview |
| Tables | No | Screenshot tables and insert as images |
| Code blocks | No | Screenshot or omit (use for illustration only) |
| `[Links](url)` | Yes (use toolbar) | Insert via link button |
| Bullet lists | Yes | Supported natively |
| `---` Dividers | No | Use blank line or heading to separate sections |

**Recommended workflow:** Render the markdown in VSCode preview or GitHub, copy the rendered output, paste into LinkedIn's article editor. Bold, italics, headings, and lists transfer. Tables and code blocks require screenshots.

### 8.4 Short Post Structure

A short LinkedIn post that introduces a blog article should follow this structure:

1. **Hook** (first 2-3 lines) — A counterintuitive claim or surprising result. This is what appears before "see more." It must generate curiosity.
2. **Context** (1-2 sentences) — What the project was and why it matters.
3. **Key results** (numbered list) — The headline findings, concise enough to scan.
4. **Bridge to article** (1 sentence) — What the full article covers beyond the post.
5. **Hashtags** (end) — 4-6 relevant tags.

**Important:** Put the article link in a **comment**, not in the post body. LinkedIn's algorithm penalizes posts with outbound links — placing the link in a comment avoids this penalty while still making it accessible.

### 8.5 Follow-Up Comment Structure

The comment on the original post should be:

1. **Signal** — "The full article is live" or equivalent
2. **Link** — Direct URL to the LinkedIn Article
3. **Value proposition** — One sentence on what the article adds beyond the short post (depth, citations, methodology)

Example from this project:

> The full technical deep-dive is here: [link]. It covers why TF-IDF beat GloVe and FastText, how each method connects to LLMs, and what the progression from word counts to transformers actually looks like -- with 15 academic references.

### 8.6 DSM Recommendation

Add to the Blog/Communication Deliverable Process (Section 7) a new step:

```
7. **Publication phase:**
   a. Publish short post (hook + key results + promise of full article)
   b. Publish LinkedIn Article (formatted from markdown draft)
   c. Comment on short post with article link
   d. Format: tables and code blocks as images, link in comment not post body
```

---

## 9. Files Produced

| File | Purpose |
|------|---------|
| `docs/blog-materials.md` | Raw materials, story arc, references, LinkedIn short post |
| `docs/blog-post-draft.md` | Final blog article (~2,700 words, 15 references) |
| `docs/dsm-feedback-blog.md` | This document — process feedback |
