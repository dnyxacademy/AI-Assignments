# 1. Call an LLM from Python

**Week 3 - Generative AI and retrieval · Mon · Easy · about 3 hours**

## What you will practise

- Tokens, the context window, and what sampling does
- Calling a chat API and keeping conversation history
- Keeping an API key out of your code

## Before you start

Finish Week 2. You need an API key from any provider, or Ollama running locally. The labs are provider-neutral.

## The task

Build a chatbot that remembers the conversation, and learn what the knobs do.

1. Get access to a model: any provider's free tier, or [Ollama](https://ollama.com/) for a local open model if you would rather not use a key at all.
2. Put the key in an environment variable, or a Colab secret. **Never write a key into a notebook cell, and never commit one.** A key pushed to a public repository is usually found and used within minutes.
3. Make one call and print the reply.
4. Count the tokens in your prompt and reply. Then work out what the whole exchange cost, even if it was free - knowing the unit economics matters the moment anything goes to real users.
5. Add **memory**: keep the message list and append each turn, so the model can answer "what did I just ask you?". Then watch what happens as the history grows - that is the context window filling up, and it is why long chats eventually need trimming or summarising.
6. Experiment with `temperature`: run the same prompt at 0, at 0.7 and at 1.5, three times each. Record what changes. Write one sentence on which setting you would use for extracting data from a document, and which for brainstorming names.
7. Make it **hallucinate on purpose**. Ask a confident factual question about something obscure or invented in your domain, and capture the reply. Then write two sentences on why this happens: the model predicts plausible text, and plausible is not the same as true. This is the reason Week 3 spends two days on retrieval.

Step 7 is the most useful thing you will do today. Make a tool lie to you while the stakes are zero.

## Example

Memory is just a growing list of messages:

```python
import os
from openai import OpenAI               # any OpenAI-compatible endpoint, Ollama included

client = OpenAI(api_key=os.environ['LLM_API_KEY'])   # from the environment, never inline

messages = [{'role': 'system', 'content': 'You are a careful agricultural advisor for Tamil Nadu.'}]

def ask(question):
    messages.append({'role': 'user', 'content': question})
    reply = client.chat.completions.create(
        model='gpt-4o-mini',
        messages=messages,            # the entire history goes every time
        temperature=0.2,
    ).choices[0].message.content
    messages.append({'role': 'assistant', 'content': reply})
    return reply

print(ask('Which crop suits a low-rainfall district?'))
print(ask('And what did I just ask you?'))     # it only knows because history was resent
```

The model is stateless. It remembers nothing between calls - you resend the whole conversation each time, which is also why a long chat gets slower and more expensive.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] The API key comes from an environment variable or secret, and is not in the notebook
- [ ] A basic call works and prints a reply
- [ ] Token counts and a cost estimate are recorded
- [ ] Memory works - the bot can answer "what did I just ask?"
- [ ] Temperature 0, 0.7 and 1.5 are each run three times and the differences recorded
- [ ] A deliberate hallucination is captured, with two sentences on why it happens

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **It works** | Reliable calls with working memory | Calls work, memory shaky | Not working |
| **Secret handling** | Key never in code or git | Kept out of git | Key committed |
| **Temperature** | All settings tested, a sound recommendation | Tested | Not explored |
| **Hallucination** | Captured, with a correct explanation | Captured | Not attempted |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [OpenAI - text generation guide](https://platform.openai.com/docs/guides/text-generation)
- [Ollama - run models locally](https://ollama.com/)
- [What are tokens? (tokenizer playground)](https://platform.openai.com/tokenizer)
