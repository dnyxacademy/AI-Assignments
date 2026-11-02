# Artificial Intelligence — Assignments

Practice assignments for the **Dnyx Academy AI** training-cum-internship: Python from scratch through to a deployed AI agent that uses tools, including one over MCP.

**No programming experience needed.** Python is taught from zero in Week 1, and the maths is explained in plain language only where it is actually needed — you will train models and understand the ideas without deriving anything.

---

## How to use this repository

These assignments are **practice**, and they are yours to learn from. Nobody is watching you do them.

The work that is **marked** is your own project, which grows each week, and your weekly submission. Those are listed in Dnyx Learn against each day's lesson. The assignments here are the drills that make that project possible.

**Everything runs in [Google Colab](https://colab.research.google.com/).** Nothing to install, and a free GPU when you need one in Week 2.

**Work through a week in order.** Each assignment assumes the one before it. "Before you start" says exactly what you need first.

**Two decisions in Week 1 shape your whole month:**

1. **Your domain** — education, health, agriculture, finance, retail, or your own approved idea. You keep it for all four weeks. Choose something you are genuinely curious about.
2. **Your dataset** — one real dataset, at least 500 rows, plus a set of documents you will use for retrieval in Week 3.

**Tick the "Done when" list yourself** before you submit anything. It is the same list your mentor checks.

---

## The assignments

### Week 1 — Python, data and first models (2–7 Nov)

A model you understand beats a complicated one you do not. [Week overview](./week-1-python-data-and-first-models/README.md)

| # | Assignment | What you practise |
| --- | --- | --- |
| 1 | [Python Basics in Colab](./week-1-python-data-and-first-models/01-python-basics/README.md) | Python, notebooks, Git |
| 2 | [NumPy, pandas and Your Dataset](./week-1-python-data-and-first-models/02-numpy-and-pandas/README.md) | DataFrames, vectorised operations |
| 3 | [Cleaning, Charts and Data Ethics](./week-1-python-data-and-first-models/03-clean-explore-and-ethics/README.md) | Missing values, Matplotlib, bias |
| 4 | [Your First Model](./week-1-python-data-and-first-models/04-your-first-model/README.md) | Train/test split, overfitting, baselines |
| 5 | [Evaluation: Beyond Accuracy](./week-1-python-data-and-first-models/05-evaluation/README.md) | Precision, recall, F1, pipelines |
| 6 | [Clustering with k-means](./week-1-python-data-and-first-models/06-clustering/README.md) | Unsupervised learning, choosing k |

### Week 2 — Neural networks and deep learning (9–14 Nov)

[Week overview](./week-2-neural-networks/README.md)

| # | Assignment | What you practise |
| --- | --- | --- |
| 1 | [A Tiny Network, by Hand](./week-2-neural-networks/01-tiny-network-by-hand/README.md) | Backpropagation in NumPy, no framework |
| 2 | [The Same Network in PyTorch](./week-2-neural-networks/02-pytorch-training-loop/README.md) | Tensors, autograd, the GPU |
| 3 | [CNNs and Transfer Learning](./week-2-neural-networks/03-cnn-and-transfer-learning/README.md) | Convolution, pretrained models, fine-tuning |
| 4 | [Embeddings, Attention and Transformers](./week-2-neural-networks/04-embeddings-and-attention/README.md) | Semantic similarity, self-attention |
| 5 | [Overfitting, Regularisation and Tracking](./week-2-neural-networks/05-regularisation-and-tracking/README.md) | Dropout, early stopping, reproducibility |
| 6 | [Deep Learning Against Your Baseline](./week-2-neural-networks/06-compare-to-baseline/README.md) | Fair comparison, honest conclusions |

### Week 3 — Generative AI and retrieval (16–21 Nov)

[Week overview](./week-3-generative-ai-and-retrieval/README.md)

| # | Assignment | What you practise |
| --- | --- | --- |
| 1 | [Call an LLM from Python](./week-3-generative-ai-and-retrieval/01-call-an-llm/README.md) | Tokens, memory, temperature, hallucination |
| 2 | [Prompt Engineering, Tested](./week-3-generative-ai-and-retrieval/02-prompt-engineering/README.md) | Few-shot, versioning, measuring prompts |
| 3 | [Structured Output You Can Trust](./week-3-generative-ai-and-retrieval/03-structured-output/README.md) | JSON mode, Pydantic validation, retries |
| 4 | [RAG: Chat with Your Own Documents](./week-3-generative-ai-and-retrieval/04-rag-basics/README.md) | Chunking, vector search, citations |
| 5 | [Making RAG Actually Good](./week-3-generative-ai-and-retrieval/05-rag-evaluation/README.md) | Evaluation, hybrid search, re-ranking |
| 6 | [Safety: Injection, Privacy and Copyright](./week-3-generative-ai-and-retrieval/06-safety-and-injection/README.md) | Prompt injection, redaction, licensing |

### Week 4 — Agents, MCP and shipping (23–28 Nov)

[Week overview](./week-4-agents-and-mcp/README.md)

| # | Assignment | What you practise |
| --- | --- | --- |
| 1 | [From Chatbot to Agent: Tool Calling](./week-4-agents-and-mcp/01-tool-calling/README.md) | Think-act-observe, tool definitions |
| 2 | [Agent Patterns, Guardrails and Traces](./week-4-agents-and-mcp/02-agent-patterns-and-traces/README.md) | Planning, budgets, tracing, evaluation |
| 3 | [Write an MCP Server](./week-4-agents-and-mcp/03-mcp-server/README.md) | Hosts, clients, servers, tools, resources |
| 4 | [Deploy It as a Service](./week-4-agents-and-mcp/04-deploy-fastapi/README.md) | FastAPI, rate limits, Spaces or Railway |
| 5 | [A README and a Demo Video](./week-4-agents-and-mcp/05-readme-and-demo/README.md) | Reporting results and failures honestly |
| 6 | [Final Project](./week-4-agents-and-mcp/06-final-project/README.md) | The whole month, shipped |

---

## Tools you will use

| For | What |
| --- | --- |
| Notebooks | Google Colab — free GPU, nothing to install |
| Python | NumPy, pandas, Matplotlib, scikit-learn, PyTorch |
| LLM access | Any provider's free tier, or [Ollama](https://ollama.com/) for local open models. Labs are provider-neutral |
| Vectors | Chroma locally, pgvector on Neon for production |
| App and API | Streamlit for the demo, FastAPI for the service |
| Hosting | Hugging Face Spaces, Railway, or Render free tier |
| Source control | Git, GitHub and pull requests, from day one |

---

## How to submit

Assignments are practice and are not individually graded. Your **weekly submission** is.

1. Work in **your own repository**, created from the template your mentor gives you. Do not push to this repository — it is read-only for learners.
2. Commit as you go. Notebooks diff badly, so write a real commit message saying what changed; `Untitled12.ipynb` helps nobody.
3. Open a **pull request** when the week's work is ready.
4. Paste the pull request link into the matching lesson in [Dnyx Learn](https://learn.dnyxacademy.com).
5. A mentor reviews it within **48 hours** and leaves comments on the pull request.

**Before you push, check you have not committed an API key.** This is the single most common serious mistake on this track. Keys live in environment variables or Colab secrets, never in a cell.

If something is incomplete, say so in the pull request description. An honest "the re-ranker made it worse and I do not know why" is worth more than silence.

---

## How you are marked

Scored out of 100. **Pass at 60** with 75 percent attendance; **merit at 85**.

Every assignment carries a rubric table with three levels:

| Level | Means |
| --- | --- |
| **Exemplary** | Does what was asked, handles the awkward cases, and you can explain your choices |
| **Adequate** | Works. The main path is right; edges are rough |
| **Needs work** | Does not run, or the requirement was not met |

Each brief also has a **Done when** checklist. Go through it yourself before submitting — it is the same list the mentor uses.

**One thing specific to this track:** several assignments are marked on reaching an *honest* conclusion, not a flattering one. "The neural network was worse than my simple baseline, and here is why" is a full-marks answer. Reporting a result you cannot reproduce is not.

---

## What each week expects of you

| | |
| --- | --- |
| Monday to Saturday | Sunday off |
| Daily | One ticket on your project, from the day's lesson |
| Saturday | Weekly quiz, and the week's submission |
| Week 2, Saturday | Mid-point review — a written review of Weeks 1 and 2 |
| Friday 27 Nov | Demo Day, five minutes each |
| Saturday 28 Nov | Final submission, by noon |

---

## Honest expectations

Four weeks is short. This is a **guided tour, not full mastery**, and we would rather say so on day one. Core machine learning gets one week and deep learning gets one, so you will train models and understand the ideas — you will not derive the maths, and you are not expected to.

You will finish able to clean a dataset, train and evaluate a model, build a retrieval system over your own documents, and ship an agent that uses tools. You will not finish a research scientist.

Each week ends with a "Go deeper" list. Not assessed: reinforcement learning, time-series forecasting, MLOps, quantisation and on-device models.

---

## Costs

Everything here can be done **free**:

- Colab's free tier, including the T4 GPU, is enough for every assignment.
- For LLM work, use a provider's free tier, or Ollama locally with no key and no bill at all.
- Chroma, Streamlit, FastAPI, Hugging Face Spaces and Railway all have free tiers that cover this.

If you do use a paid key, set a **spending limit on the provider's dashboard** before Week 4. An agent with no iteration cap can spend a lot overnight, and [assignment 2 of Week 4](./week-4-agents-and-mcp/02-agent-patterns-and-traces/README.md) makes you build the guardrail that prevents it.

---

## Which assignments did my batch get?

The set is maintained in place and improved between batches, so a tag marks what each batch received:

| Tag | Batch |
| --- | --- |
| `nov-2026` | November 2026 — the first batch on this track |

```bash
git checkout nov-2026     # the exact set your batch was given
```

---

## For trainers

Keep one source of truth per piece of content. The **Dnyx Learn lesson** carries the day's ticket, the due date and the submission; the **brief here** carries the long task, the worked example and the rubric, and the lesson links to it. Do not copy a brief into both.

The briefs are deliberately provider-neutral on LLMs and have no solutions committed — learners build from scratch. If solutions are ever added, they belong in a private repository.

When you change a brief, change it here and let the lesson keep pointing at it. Tag at the start of each batch.

See also: [MERN-Assignments](https://github.com/dnyxacademy/MERN-Assignments) for the web development track.

---

Questions go in the cohort chat, not in an issue on this repository.
