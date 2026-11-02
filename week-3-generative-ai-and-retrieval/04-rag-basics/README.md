# 4. RAG: Chat with Your Own Documents

**Week 3 - Generative AI and retrieval · Wed · Hard · about 4 hours**

## What you will practise

- Chunking, embedding and vector search
- Grounding an answer in retrieved text
- Citations, so an answer can be checked

## Before you start

Finish 03 and the Week 2 embeddings assignment. Workshop day. Gather your domain documents first.

## The task

Yesterday the model invented facts. Today you give it the facts, and make it show its sources.

1. Collect **at least ten documents** about your domain - PDFs, web pages, reports. These stay your document set for the rest of the programme.
2. Extract the text. PDFs are messier than they look: check what you actually got, because tables and multi-column layouts often come out scrambled.
3. **Chunk** the text. Start at roughly 500 characters with a 50-character overlap. The overlap stops a sentence that straddles a boundary from being lost to both chunks.
4. Embed every chunk and store the vectors in Chroma.
5. Build retrieval: a question comes in, and the five closest chunks come back. Print them and read them - if retrieval returns the wrong chunks, no prompt will save the answer. **Debug retrieval before you blame the model.**
6. Build the full loop: retrieve, put the chunks into the prompt as context, instruct the model to answer **only** from that context and to say "I don't know" when the context does not cover it.
7. Add **citations**: every answer names which document and chunk it came from. An answer nobody can verify is not much better than a guess.
8. Test the refusal. Ask something your documents genuinely do not cover and confirm it declines instead of inventing. Compare that with yesterday's hallucination.

Step 8 is the point of the whole week.

## Example

Chunking with overlap, and why:

```python
def chunk(text, size=500, overlap=50):
    out, start = [], 0
    while start < len(text):
        out.append(text[start:start + size])
        start += size - overlap        # step back a little, so boundaries are covered twice
    return out
```

Grounding the prompt so refusal is possible:

```python
PROMPT = '''Answer the question using ONLY the context below.
If the context does not contain the answer, reply exactly: "I don't know from these documents."
Cite the source of each fact as [source: filename, chunk N].

Context:
{context}

Question: {question}'''
```

```python
hits = collection.query(query_texts=[question], n_results=5)

# read these before trusting any answer - bad retrieval looks like a bad model
for doc, meta in zip(hits['documents'][0], hits['metadatas'][0]):
    print(meta['filename'], meta['chunk'], doc[:120])

context = '\n\n'.join(
    f"[source: {m['filename']}, chunk {m['chunk']}]\n{d}"
    for d, m in zip(hits['documents'][0], hits['metadatas'][0])
)
```

The instruction to reply with an exact refusal phrase is deliberate: it gives you something you can test for automatically tomorrow.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] At least ten real domain documents are collected and listed
- [ ] Extracted text is checked, and extraction problems noted
- [ ] Chunking uses an overlap, and the size is stated
- [ ] Chunks are embedded and stored in a vector database
- [ ] Retrieved chunks are printed and inspected for relevance
- [ ] Answers are grounded in the retrieved context only
- [ ] Every answer carries a citation naming document and chunk
- [ ] An uncovered question produces a refusal, not an invention

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Pipeline** | Full retrieve-then-generate loop working | Partly working | Not working |
| **Chunking** | Sensible size and overlap, reasoned | Chunked | Whole documents, or no overlap |
| **Citations** | Every fact traceable to a chunk | Some citations | None |
| **Refusal** | Declines when ungrounded, demonstrated | Sometimes declines | Always answers |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [Chroma - getting started](https://docs.trychroma.com/getting-started)
- [The original RAG paper](https://arxiv.org/abs/2005.11401)
- [LangChain - RAG tutorial](https://python.langchain.com/docs/tutorials/rag/)
