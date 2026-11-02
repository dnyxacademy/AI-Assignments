# 1. From Chatbot to Agent: Tool Calling

**Week 4 - Agents, MCP and shipping · Mon · Medium · about 3-4 hours**

## What you will practise

- Think, act, observe
- Defining tools a model can call
- Judging when an agent is the wrong answer

## Before you start

Finish Week 3. You need a working LLM call and your RAG pipeline.

## The task

A chatbot answers. An agent decides to *do* something, does it, looks at the result, and decides again.

1. Define **two tools** the model may call. Make them real and useful in your domain, for example:
   - a calculator, or a domain-specific computation
   - a lookup against your RAG documents
   - a public API your domain cares about - weather, prices, a public dataset
2. Describe each tool to the model: name, what it does, its parameters and their types. **The description is the interface.** A vague description is the single commonest reason a model calls the wrong tool or fills arguments wrongly - treat it as carefully as you would an API doc.
3. Build the loop: send the question with the tool definitions; if the model asks for a tool, run it, send the result back, and repeat until it answers.
4. Cap the iterations. An agent that loops forever is the standard first bug, and it costs real money per turn.
5. Log every step - which tool, what arguments, what came back. You cannot debug an agent you cannot see.
6. Test three cases: one needing no tool, one needing one tool, one needing both in sequence.
7. Answer honestly: **would a plain function have been better here?** For a fixed sequence of steps, a normal program is cheaper, faster and more reliable. An agent earns its cost only when the steps are genuinely not known in advance. Say which yours is.

Step 7 is marked. "An agent was unnecessary, and here is why" is a correct answer.

## Example

A tool definition. The description is doing the real work:

```python
tools = [{
    'type': 'function',
    'function': {
        'name': 'lookup_documents',
        'description': (
            'Search the agricultural advisory documents for factual guidance. '
            'Use this for any question about crops, irrigation, pests or soil. '
            'Do NOT use it for arithmetic or for current weather.'      # boundaries matter
        ),
        'parameters': {
            'type': 'object',
            'properties': {
                'query': {'type': 'string', 'description': 'The search phrase'},
            },
            'required': ['query'],
        },
    },
}]
```

The loop, with the cap that saves you money:

```python
messages = [{'role': 'user', 'content': question}]

for step in range(5):                        # hard cap: never trust it to stop
    reply = client.chat.completions.create(
        model='gpt-4o-mini', messages=messages, tools=tools,
    ).choices[0].message
    messages.append(reply)

    if not reply.tool_calls:
        print(reply.content); break          # it has an answer

    for call in reply.tool_calls:
        args = json.loads(call.function.arguments)
        print('TOOL', call.function.name, args)      # log every action
        result = TOOLS[call.function.name](**args)
        messages.append({'role': 'tool', 'tool_call_id': call.id, 'content': str(result)})
else:
    print('Gave up after 5 steps.')
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] Two real tools are defined with clear descriptions and typed parameters
- [ ] The think-act-observe loop works end to end
- [ ] Iterations are capped, and the cap is tested
- [ ] Every step is logged with tool, arguments and result
- [ ] All three test cases pass: no tool, one tool, two tools in sequence
- [ ] You answered whether a plain function would have been better, with reasoning

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Tools** | Both genuinely useful, descriptions precise | Both work | One, or poorly described |
| **The loop** | Correct, capped, handles multiple calls | Works | Loops or breaks |
| **Logging** | Every step visible and debuggable | Some logging | None |
| **Judgement** | Clear-eyed about whether an agent was needed | Partly | Not considered |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [OpenAI - function calling](https://platform.openai.com/docs/guides/function-calling)
- [ReAct: reasoning and acting (paper)](https://arxiv.org/abs/2210.03629)
- [Anthropic - building effective agents](https://www.anthropic.com/engineering/building-effective-agents)
