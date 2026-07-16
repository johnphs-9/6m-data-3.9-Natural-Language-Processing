# L09 NLP, Embeddings & Semantic Search — Multiple Choice Questions

Test your understanding of the key concepts. The correct answer is indicated below each question.

---

**Q1. A shopper searches "blue summer dress" but the matching product is described as a "lightweight floral frock." Why does keyword search fail to return it?**

- A. The query has too many stop-words
- B. Keyword search treats tokens as identities, not meanings — *frock* and *dress* are different tokens with no relationship
- C. The product description is too long
- D. Stemming converts *dress* into an unsearchable root

**✅ Correct answer: B**

---

**Q2. What is the cosine similarity between two distinct one-hot vectors (e.g. `onehot("frock")` and `onehot("dress")`)?**

- A. Exactly 1.0
- B. Around 0.25
- C. Exactly 0.0
- D. It depends on how semantically related the words are

**✅ Correct answer: C** — One-hot vectors for distinct words share no non-zero positions, so their dot product (and cosine) is exactly 0 regardless of meaning.

---

**Q3. What best defines an embedding?**

- A. A sparse vector of word counts indexed by a fixed vocabulary
- B. A dense vector where geometric distance encodes semantic distance
- C. A one-hot vector with a single 1 at the word's index
- D. A percentage score of how similar two words are

**✅ Correct answer: B**

---

**Q4. Which principle provides the training signal for embeddings — that a word's meaning comes from the contexts it appears in?**

- A. The bag-of-words assumption
- B. The orthogonality principle
- C. The distributional hypothesis ("you shall know a word by the company it keeps")
- D. The TF-IDF weighting rule

**✅ Correct answer: C**

---

**Q5. With `all-MiniLM-L6-v2`, cosine(*frock*, *dress*) ≈ 0.25. A colleague says "only 25% similar — not very high." Are they right?**

- A. Yes, ≈0.25 means 25% similar
- B. Yes, anything below 0.5 is a poor match
- C. No — cosine is a ranking signal, not a percentage; even unrelated pairs score ~0.25, so a single pairwise value means little on its own
- D. No, because cosine values are always negative

**✅ Correct answer: C**

---

**Q6. Why does NB 03 use a pretrained sentence-transformer rather than averaging individual word vectors across a sentence?**

- A. Averaging is computationally impossible
- B. Averaging throws away word order and dilutes high-information words; a sentence-transformer captures the whole sentence's meaning
- C. Word vectors cannot be averaged mathematically
- D. Sentence-transformers are always faster than averaging

**✅ Correct answer: B** — e.g. averaging gives "dog bites man" and "man bites dog" identical embeddings.

---

**Q7. Which statement about pretrained sentence-transformers like `all-MiniLM-L6-v2` is correct?**

- A. You must train them from scratch on your own data
- B. They require labels and a fine-tuning loop before use
- C. They are the default starting point — you call `.encode()` and store the result; the model does the heavy lifting
- D. They output sparse one-hot vectors

**✅ Correct answer: C** — 22M params, 384-dim output, trained on >1B sentence pairs.

---

**Q8. What are the two core functions of a semantic search engine?**

- A. Train a model + fine-tune on labels
- B. Embed the corpus once (offline) + cosine-against-the-query (at request time), returning top-K
- C. Stem all words + remove stop-words
- D. Count token frequencies + apply TF-IDF weighting

**✅ Correct answer: B** — No labels, no fine-tuning, no training loop.

---

**Q9. On NorthStar's 8-query benchmark, the semantic engine scored top-1 = 6/8 and top-5 = 7/8. How should these metrics be interpreted?**

- A. Top-1 measures the strict "best result first" experience; top-5 reflects realistic browsing — report both, and the gap is recoverable headroom via re-ranking
- B. Only top-1 matters; top-5 is meaningless
- C. The system failed because neither metric is 8/8
- D. Top-5 should always be lower than top-1

**✅ Correct answer: A**

---

**Q10. Why is production search hybrid (TF-IDF + embeddings + filters) rather than embeddings-only?**

- A. Embeddings cannot be stored in a database
- B. TF-IDF wins on exact-token queries (product codes, brand names, SKUs) where embeddings are weak, and structured filters enforce hard constraints like category and price
- C. Embeddings are too slow to compute at any scale
- D. TF-IDF always outperforms embeddings on every query type

**✅ Correct answer: B** — Standard pattern: union of TF-IDF + semantic candidates → structured filters → re-rank by score blend, popularity, recency.

---

### Bonus concept: stale embeddings

When a merchandiser rewrites a product description, its cached embedding is computed from the *old* text and goes stale until re-encoded — the standard fix is a nightly re-embedding job for any row whose `updated_at` changed in the last 24 hours.
