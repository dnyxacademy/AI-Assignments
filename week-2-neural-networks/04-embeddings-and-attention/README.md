# 4. Embeddings, Attention and Transformers

**Week 2 - Neural networks and deep learning · Thu · Medium · about 3-4 hours**

## What you will practise

- Embeddings: meaning as a vector
- RNNs and LSTMs in outline, and their limitation
- Attention, and why transformers replaced them

## Before you start

Finish 03. This is the bridge to Week 3 - everything about LLMs rests on it.

## The task

Build an intuition for embeddings, then understand what attention solved.

1. Load a pretrained sentence embedding model (`sentence-transformers`, for example `all-MiniLM-L6-v2`).
2. Embed ten sentences from your domain, then compute cosine similarity between every pair. Show it as a heatmap.
3. Check it behaves sensibly: sentences meaning similar things should score high even with no shared words, and sentences sharing words but not meaning should score lower. Find one pair where it gets this **wrong** and show it. Embeddings are good, not perfect, and seeing a failure now will save you hours of confusion in Week 3's RAG assignment.
4. Build a tiny semantic search: given a query, return the three closest of your sentences. This is the core of RAG, in about ten lines.
5. Write short explanations, in your own words:
   - why an RNN struggles with a long sentence
   - what attention lets the model do instead
   - what "self-attention" means
   - why transformers can be trained in parallel while RNNs cannot - this is the reason they won
6. Draw the transformer architecture by hand - paper and a photo is fine - and label: embedding, positional encoding, attention, feed-forward, and the output head. Explain in two sentences why positional encoding is needed at all.

Step 6 sounds like busywork. Drawing it once is the difference between recognising the diagram and understanding it.

## Example

Semantic search, which is all RAG retrieval really is:

```python
from sentence_transformers import SentenceTransformer, util

model = SentenceTransformer('all-MiniLM-L6-v2')

docs = [
    'Rice needs standing water during early growth.',
    'Wheat is sown after the monsoon retreats.',
    'Drip irrigation reduces water use in dry districts.',
]
doc_vectors = model.encode(docs, convert_to_tensor=True)

query = 'how do I use less water?'
hits = util.semantic_search(model.encode(query, convert_to_tensor=True), doc_vectors, top_k=3)

for hit in hits[0]:
    print(round(hit['score'], 3), docs[hit['corpus_id']])
# 0.571 Drip irrigation reduces water use in dry districts.   <- no shared words with the query
```

The top hit shares not one word with the query. That is the whole point of embeddings: closeness in meaning, not in spelling.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] Ten domain sentences are embedded and a similarity heatmap is shown
- [ ] A pair with high similarity and no shared words is identified
- [ ] A pair the model scores **wrongly** is found and shown
- [ ] A working top-3 semantic search over your sentences
- [ ] All four written explanations are in your own words
- [ ] A hand-drawn, labelled transformer diagram is committed, with positional encoding explained

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Embeddings** | Similarity behaves sensibly and is demonstrated | Computed | Not working |
| **Finding a failure** | A real failure case found and discussed | Attempted | Not attempted |
| **Semantic search** | Returns sensible results | Runs | Broken |
| **Understanding attention** | All four explanations correct, parallelism included | Mostly right | Copied jargon |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
- [Attention is all you need (the original paper)](https://arxiv.org/abs/1706.03762)
- [sentence-transformers docs](https://sbert.net/)
