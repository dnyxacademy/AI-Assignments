# 4. Deploy It as a Service

**Week 4 - Agents, MCP and shipping · Thu · Medium · about 3-4 hours**

## What you will practise

- Wrapping a model pipeline in a FastAPI service
- Secrets, rate limits and logging
- Deploying to Hugging Face Spaces or Railway

## Before you start

Finish 03. You need a working agent or RAG pipeline.

## The task

A notebook is not a product. Put your work behind an API and a small UI, and get it online.

1. Wrap your pipeline in **FastAPI**. At minimum: `POST /ask` takes a question and returns an answer with its citations, and `GET /health` returns `{"status": "ok"}`.
2. Validate the request body with Pydantic. Cap the question length - without a cap, one huge request can exhaust your whole budget.
3. Read every secret from the environment. Commit a `.env.example` with the names and no values.
4. Add a **rate limit** per caller. A public endpoint calling a paid model with no limit is a bill waiting to happen, and this is the most common way a student project becomes expensive.
5. Log every request: question, latency, tokens, cost, whether it succeeded. **Do not log personal data**, and do not log full document contents.
6. Build a small **Streamlit** interface for the demo - a text box, the answer, and the citations shown as sources.
7. Deploy to **Hugging Face Spaces** or **Railway**. Set the secrets in the platform, not in the code.
8. **Then test the deployed URL** - not localhost. Confirm `/health` answers, one real question works, and the rate limit triggers when you exceed it. A green deploy is not evidence the thing works.
9. Put the live URL in your README.

## Example

A validated endpoint with a cap:

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, Field

app = FastAPI()

class Question(BaseModel):
    text: str = Field(min_length=3, max_length=500)   # the cap matters: cost control

class Answer(BaseModel):
    answer: str
    sources: list[str]

@app.get('/health')
def health():
    return {'status': 'ok'}

@app.post('/ask', response_model=Answer)
def ask(q: Question):
    try:
        result = pipeline(q.text)
    except BudgetExhausted:
        raise HTTPException(status_code=429, detail='Budget exhausted, try later')
    return Answer(answer=result.text, sources=result.sources)
```

Logging the numbers you will want later, and nothing you should not keep:

```python
logger.info(
    'ask ok=%s latency_ms=%d tokens=%d cost_usd=%.5f q_len=%d',
    ok, latency_ms, tokens, cost, len(q.text),      # length, not the question text
)
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] `POST /ask` and `GET /health` both work
- [ ] The request body is validated and the question length is capped
- [ ] Every secret comes from the environment, with a `.env.example` committed
- [ ] A rate limit is enforced per caller and you triggered it on purpose
- [ ] Requests are logged with latency, tokens, cost and outcome, and no personal data
- [ ] A Streamlit interface shows the answer and its sources
- [ ] The service is deployed and the live URL is in the README
- [ ] You tested the deployed URL, not localhost, and said how

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **The service** | Both endpoints correct and validated | Works | Not working |
| **Cost and safety** | Rate limit and caps enforced and tested | Rate limit present | Unlimited |
| **Deployment** | Live, secrets set on the platform, verified | Deployed | Local only |
| **Observability** | Useful logs, nothing sensitive kept | Some logs | None |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [FastAPI - first steps](https://fastapi.tiangolo.com/tutorial/first-steps/)
- [Streamlit - get started](https://docs.streamlit.io/get-started)
- [Hugging Face Spaces](https://huggingface.co/docs/hub/spaces)
- [Railway - deploy a FastAPI app](https://docs.railway.com/guides/fastapi)
