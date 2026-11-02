# 2. Prompt Engineering, Tested

**Week 3 - Generative AI and retrieval · Tue · Medium · about 3 hours**

## What you will practise

- Roles, examples and step-by-step reasoning
- Writing prompts you can version and test
- Measuring whether a prompt change actually helped

## Before you start

Finish 01. You need a working LLM call.

## The task

Treat a prompt as code: write it down, version it, and test it.

1. Pick a real task in your domain with a checkable answer - classify a complaint, pull fields out of a form, judge sentiment.
2. Write **ten test cases** with the answer you expect, before you write any prompt. This is the part that makes the rest meaningful.
3. Write **four versions** of the prompt and score each against all ten cases:
   - **v1 zero-shot**: just the instruction
   - **v2 with a role and constraints**: who the model is, what it must and must not do
   - **v3 few-shot**: two or three worked examples inside the prompt
   - **v4 step-by-step**: ask it to reason before answering
4. Build a results table: version, how many of ten correct, what it got wrong.
5. Pick a winner from the numbers, not from which prompt feels cleverest. Few-shot often beats elaborate instructions, and step-by-step can make a simple classification *worse* by talking it out of the obvious answer.
6. Store the winning prompt in a file with a version number and a comment on why it won. Prompts drift; an undocumented one nobody can reproduce is a liability.
7. Find one input where even the best version fails, and say what you would do about it.

A prompt without test cases is a guess. The ten cases are the assignment.

## Example

Four versions of the same task:

```python
# v1 - zero-shot
'Classify this complaint: {text}'

# v2 - role and constraints
'''You are a support triage assistant for an agricultural helpline.
Classify the complaint into exactly one of: PEST, IRRIGATION, SEED_QUALITY, OTHER.
Reply with the single label and nothing else.

Complaint: {text}'''

# v3 - few-shot: show, do not only tell
'''Classify into PEST, IRRIGATION, SEED_QUALITY or OTHER.

Complaint: "Leaves have holes and I see insects"  -> PEST
Complaint: "Canal water has not come for ten days" -> IRRIGATION
Complaint: "Only half the seeds sprouted"          -> SEED_QUALITY

Complaint: {text} ->'''
```

And a results table that justifies the choice:

```markdown
| Version | Correct /10 | Where it failed |
| --- | --- | --- |
| v1 zero-shot | 6 | Invented labels like "WATER"; prose instead of a label |
| v2 role + constraints | 8 | Both failures were genuinely ambiguous complaints |
| v3 few-shot | **9** | One pest-and-irrigation complaint, only one label allowed |
| v4 step-by-step | 7 | Reasoned itself out of two obvious PEST cases |

Winner: v3. The remaining failure is a real limitation of single-label
classification, not a prompt problem - it needs multi-label output.
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] Ten test cases with expected answers were written **before** the prompts
- [ ] All four prompt versions are run against all ten cases
- [ ] A results table records score and failure mode per version
- [ ] The winner is chosen from the scores, not from preference
- [ ] The winning prompt is saved in a versioned file with a comment on why
- [ ] One remaining failure case is identified, with a proposed next step

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Test cases** | Ten real cases written first, answers clear | Some cases | None, or written afterwards |
| **Prompt variety** | All four genuinely different techniques | Two or three | One |
| **Measurement** | Every version scored, failures described | Scored | Judged by feel |
| **Versioning** | Prompt stored with version and reasoning | Saved | Lost in a notebook cell |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [OpenAI - prompt engineering guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [Anthropic - prompt engineering overview](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Chain-of-thought prompting (paper)](https://arxiv.org/abs/2201.11903)
