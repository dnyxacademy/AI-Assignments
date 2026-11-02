# 2. Agent Patterns, Guardrails and Traces

**Week 4 - Agents, MCP and shipping · Tue · Hard · about 4 hours**

## What you will practise

- Planning, reflection, routing and memory
- Guardrails, cost control and tracing
- Evaluating an agent, which is harder than evaluating a model

## Before you start

Finish 01. Build your own loop first - then, and only then, try a framework.

## The task

Add a third tool, then make the agent observable and safe enough to trust.

1. Add a **third tool**, one with a side effect - writing a file, sending a message to a test channel, appending to a sheet. Anything that acts on the world needs more care than anything that only reads.
2. Implement one of these patterns and say why you chose it:
   - **planning**: write the steps first, then execute them
   - **reflection**: produce an answer, criticise it, revise
   - **routing**: classify the request, send it to the right specialist path
3. Add **guardrails**:
   - a token or rupee budget per request, enforced
   - a confirmation step before any tool with a side effect runs
   - a check on tool arguments before execution - never pass a model's output straight into a shell command, an SQL query or a file path
4. Add **tracing**: every run records each step, its inputs, outputs, tokens, latency and cost. Use a tool like LangSmith or Phoenix, or write a JSONL log yourself.
5. Build a **ten-case evaluation set** and define what success means per case. Agent evaluation is hard because several different paths can be correct - score the outcome, not the route.
6. Run all ten, report the pass rate and the average cost and latency per run.
7. Now try a framework - LangGraph or an agent SDK - for the same task, and write three sentences comparing it with your own loop. You will understand what the framework gives you precisely because you built it yourself first.

Guardrail 3 is the one that matters most. Treat a model's output as untrusted user input, always.

## Example

Validating arguments before acting:

```python
from pathlib import Path

SAFE_DIR = Path('./agent_output').resolve()

def write_report(filename: str, content: str):
    # NEVER do: open(filename, 'w')  -- the model could pass ../../.env
    target = (SAFE_DIR / Path(filename).name).resolve()
    if not target.is_relative_to(SAFE_DIR):
        raise ValueError('refused: path escapes the output directory')
    if len(content) > 50_000:
        raise ValueError('refused: content too large')
    target.write_text(content, encoding='utf-8')
    return f'wrote {target.name}'
```

A budget, enforced rather than hoped for:

```python
class Budget:
    def __init__(self, max_tokens): self.used, self.cap = 0, max_tokens
    def charge(self, n):
        self.used += n
        if self.used > self.cap:
            raise RuntimeError(f'budget exhausted: {self.used}/{self.cap} tokens')

budget = Budget(20_000)     # one runaway agent can otherwise spend all night
```

One trace line per step, so a failure can be reconstructed:

```json
{"run":"a91f","step":2,"tool":"lookup_documents","args":{"query":"drip irrigation"},
 "tokens":412,"latency_ms":680,"cost_usd":0.00009,"ok":true}
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] A third tool with a side effect exists and is used
- [ ] One of planning, reflection or routing is implemented, with the choice explained
- [ ] A budget is enforced and tested by deliberately exceeding it
- [ ] Side-effect tools require confirmation before running
- [ ] Tool arguments are validated - a path-escape or injection attempt is refused, and you tested it
- [ ] Traces record step, inputs, outputs, tokens, latency and cost
- [ ] A ten-case evaluation set is run, with pass rate, average cost and latency reported
- [ ] A framework is tried and compared with your own loop in three sentences

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Pattern** | Implemented well and justified | Implemented | None |
| **Guardrails** | All three, and each tested | One or two | None |
| **Tracing** | Full traces, failures reconstructable | Some logging | None |
| **Evaluation** | Ten cases, outcome-scored, costs reported | Some cases | Not evaluated |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [LangGraph - agent orchestration](https://langchain-ai.github.io/langgraph/)
- [Arize Phoenix - LLM tracing](https://docs.arize.com/phoenix)
- [Anthropic - building effective agents](https://www.anthropic.com/engineering/building-effective-agents)
