# 5. A README and a Demo Video

**Week 4 - Agents, MCP and shipping · Thu · Easy · about 2-3 hours**

## What you will practise

- Documenting an AI project, including what it gets wrong
- Reporting results honestly
- Demonstrating your work in two minutes

## Before you start

Finish 04. Your service should be deployed.

## The task

An AI project needs one thing a normal project does not: a straight account of how often it is wrong.

1. Write the README:
   - one sentence on what it does and who for
   - a screenshot or GIF near the top
   - the live demo link
   - architecture: a simple diagram of data in, retrieval, model, answer out
   - how to run it locally, tested on a clean clone
   - environment variables, as a table of name and purpose, never values
   - **Results**: your evaluation numbers from Week 3's fifteen questions and Week 4's ten agent cases. Real numbers, not "works well"
   - **Failure analysis**: at least three things it gets wrong, with examples
   - **Limitations**: what it cannot do, what you would build next
2. Write the **How I used AI** note: where an assistant helped, where it was confidently wrong, one bug it introduced that you caught, and what you now check before accepting a suggestion.
3. Test your setup instructions on a clean clone in a new folder, with a fresh virtual environment.
4. Record two minutes: the problem, the live demo, one thing you are proud of, one thing you would change. Speak over it.
5. Put the video link in the README.

The failure analysis is the most valuable section in an AI project. Anyone can show three questions that work. Knowing where your system breaks, and saying so, is what makes the rest of your numbers believable.

## Example

The two sections that distinguish an AI README:

```markdown
## Results

Evaluated on 15 held-out questions (10 single-document, 3 multi-document,
2 unanswerable) and 10 agent cases.

| Measure | Score |
| --- | --- |
| Retrieval @5 | 14/15 |
| Answer correct | 12/15 |
| Correct refusal on unanswerable | 2/2 |
| Agent task success | 8/10 |
| Median latency | 1.4 s |
| Average cost per question | ₹0.08 |

## Failure analysis

1. **Multi-document questions.** Both remaining errors need facts from two
   documents. Retrieval finds both chunks; the model answers from one and
   ignores the other. A re-ranker did not help - this is a generation problem.
2. **Numbers in tables.** PDF table extraction scrambles columns, so figures
   are sometimes attached to the wrong row. Any numeric answer should be
   checked against the source. This is the most dangerous failure here,
   because the answer looks confident and cites a real document.
3. **Follow-up questions.** "What about in winter?" loses the subject of the
   previous turn, because the retrieval query is built from the latest
   message alone rather than the conversation.
```

Note that failure 2 explains *why* it is the dangerous one. That judgement is what is being marked.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] The README opens with one clear sentence on what it does
- [ ] A screenshot or GIF is near the top
- [ ] The live demo link works when clicked
- [ ] An architecture diagram shows the flow
- [ ] Setup instructions were tested on a clean clone with a fresh environment
- [ ] Environment variables are listed by name and purpose, with no values
- [ ] Real evaluation numbers are reported, not adjectives
- [ ] At least three failure modes are described with examples
- [ ] A "How I used AI" note names a specific case of the assistant being wrong
- [ ] A two-minute video is recorded and linked

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Clarity** | A stranger understands it in 30 seconds | Understandable | Confusing |
| **Results** | Real numbers from a real test set | Some numbers | Adjectives only |
| **Failure analysis** | Three failures with examples and insight | Mentioned | Absent |
| **Video** | Clear, two minutes, shows the real thing | Recorded | None |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [Make a README](https://www.makeareadme.com/)
- [Model cards for model reporting (paper)](https://arxiv.org/abs/1810.03993)
- [Hugging Face - model card guide](https://huggingface.co/docs/hub/model-cards)
