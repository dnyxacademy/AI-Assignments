# Week 3 — Generative AI and retrieval

**16–21 November**

Large language models: what they are, how to make them useful, and how to stop them inventing things. You will make a model hallucinate on purpose on Monday, then spend the rest of the week building a system that does not.

## What you will learn this week

- Tokens, context windows, sampling, and why hallucination happens
- Prompt engineering you can version and test
- Structured output, validated with Pydantic
- RAG: chunking, embedding, retrieval, grounding, citations
- Measuring and improving a RAG system
- Prompt injection, privacy, and copyright

## The assignments

| # | Assignment | Day | Level | Hours |
| --- | --- | --- | --- | --- |
| 1 | [Call an LLM from Python](./01-call-an-llm/README.md) | Mon | Easy | 3 |
| 2 | [Prompt Engineering, Tested](./02-prompt-engineering/README.md) | Tue | Medium | 3 |
| 3 | [Structured Output You Can Trust](./03-structured-output/README.md) | Tue | Medium | 3 |
| 4 | [RAG: Chat with Your Own Documents](./04-rag-basics/README.md) | Wed | Hard | 4 |
| 5 | [Making RAG Actually Good](./05-rag-evaluation/README.md) | Thu | Hard | 4 |
| 6 | [Safety: Injection, Privacy and Copyright](./06-safety-and-injection/README.md) | Fri | Medium | 3 |

## This week’s submission

A Streamlit assistant that answers from your own documents with citations, plus an evaluation table, a pull request and a week note — by Saturday 21 November. There is a **catch-up hour** on Saturday.

## Read this before you start

**Never commit an API key.** A key pushed to a public repository is typically found and used within minutes, and the bill is yours. Use an environment variable or a Colab secret from the first cell onwards.

The other trap is debugging the wrong half. When a RAG answer is wrong, print the retrieved chunks first. Most "the model is bad" problems are retrieval returning the wrong text, and no amount of prompt tuning fixes that.

## The AI strand this week

This whole week is the AI strand. The one rule that does not bend: never paste a key, a secret, or someone’s personal data into a prompt.

---

[All weeks](../README.md) · [How to submit](../README.md#how-to-submit) · [How you are marked](../README.md#how-you-are-marked)
