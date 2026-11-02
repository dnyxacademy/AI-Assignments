# 3. Structured Output You Can Trust

**Week 3 - Generative AI and retrieval · Tue · Medium · about 3 hours**

## What you will practise

- Making a model return valid JSON every time
- Validating the output with Pydantic
- Handling the case where it still gets it wrong

## Before you start

Finish 02. You need your winning prompt.

## The task

A model that returns prose is a demo. A model that returns a validated object is something you can put in a program.

1. Build an **extractor**: text in your domain goes in - a complaint, an invoice, a report - and a fixed set of fields comes out.
2. Define the shape with a **Pydantic model**. Give every field a type, and allow `None` where a value may genuinely be absent.
3. Use the provider's structured-output or JSON mode if it has one. It is far more reliable than asking politely in the prompt.
4. **Validate every response.** Never trust the output just because it parsed - a model can return valid JSON with a field invented or a date in the wrong format.
5. Handle failure properly: retry once with the validation error fed back into the prompt, and if it fails again, record it rather than crashing. Write down how often it failed across twenty inputs.
6. Run twenty real inputs through it and report: how many validated first time, how many needed the retry, how many never worked.
7. Add one field the source text usually does **not** contain, and check the model returns `None` rather than inventing something. A model that fabricates a missing value is worse than one that admits it does not know.

Step 7 is the test that separates a usable extractor from a dangerous one.

## Example

Describe the shape, then enforce it:

```python
from pydantic import BaseModel, Field
from typing import Literal

class Complaint(BaseModel):
    category: Literal['PEST', 'IRRIGATION', 'SEED_QUALITY', 'OTHER']
    severity: int = Field(ge=1, le=5)
    district: str | None = None          # often absent - must come back as None
    crop: str | None = None
    summary: str = Field(max_length=200)
```

```python
from pydantic import ValidationError

def extract(text, attempt=1):
    raw = call_model(text)                      # your provider's JSON mode
    try:
        return Complaint.model_validate_json(raw)
    except ValidationError as e:
        if attempt == 1:
            # hand the model its own error and let it correct itself
            return extract(text + '\n\nYour last reply was invalid: ' + str(e), attempt=2)
        logging.warning('extraction failed after retry: %s', e)
        return None                             # record it, do not crash the pipeline
```

`severity: int = Field(ge=1, le=5)` is doing real work: the model will sometimes return 7, or the string "high", and this is what catches it.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] A Pydantic model defines every field with a type, and optional fields allow `None`
- [ ] Structured-output or JSON mode is used where the provider offers it
- [ ] Every response is validated, not just parsed
- [ ] A retry feeds the validation error back, and a second failure is recorded rather than crashing
- [ ] Twenty inputs are run, with first-time, retry and never-worked counts reported
- [ ] A usually-absent field returns `None` instead of an invented value

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Schema** | Complete, correctly typed, optionals honest | Defined | Loose or untyped |
| **Validation** | Every response validated and enforced | Parsed only | Trusted blindly |
| **Failure handling** | Retry plus logging, never crashes | Retries | Crashes on bad output |
| **No fabrication** | Missing fields come back `None`, demonstrated | Mostly | Invents values |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [OpenAI - structured outputs](https://platform.openai.com/docs/guides/structured-outputs)
- [Pydantic - models](https://docs.pydantic.dev/latest/concepts/models/)
- [Instructor - structured LLM output](https://python.useinstructor.com/)
