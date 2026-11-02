# 1. Python Basics in Colab

**Week 1 - Python, data and first models · Mon · Easy · about 2-3 hours**

## What you will practise

- Python: variables, lists, dicts, loops, functions
- Google Colab notebooks, and the free GPU
- Git and GitHub from a notebook

## Before you start

Nothing. Python is taught from scratch here. You need a Google account for Colab and a GitHub account.

## The task

Get your tools working and write your first Python.

1. Open [Google Colab](https://colab.research.google.com/) and make a new notebook. Nothing to install - it runs in the browser.
2. Work through these in separate cells, each with a short markdown note above it saying what it does:
   - a variable of each basic type: `int`, `float`, `str`, `bool`
   - a list of at least five items, and printing the third one
   - a dict describing something real from your chosen domain, with at least four keys
   - a `for` loop over your list
   - an `if`/`elif`/`else` that makes a decision
   - a function that takes two arguments and **returns** a value, rather than printing it
3. Check the GPU is available: `Runtime > Change runtime type > T4 GPU`, then run the cell in the example below. You will need this in Week 2.
4. Create a GitHub repository for the programme and commit this notebook to it.
5. Pick your **domain** for the whole month - education, health, agriculture, finance, retail, or your own idea - and write a markdown cell saying which, and one problem in it you would like to solve. You keep this domain for all four weeks.

Choose a domain you find genuinely interesting. You will be looking at the same data for four weeks, and curiosity is what carries you through Week 3.

## Example

Printing versus returning catches almost everyone on day one:

```python
def add_wrong(a, b):
    print(a + b)        # prints to the screen, gives back None

def add_right(a, b):
    return a + b        # hands the value back, so you can use it

total = add_right(2, 3) * 10     # 50
broken = add_wrong(2, 3) * 10    # TypeError: cannot multiply NoneType
```

Confirming the GPU is on:

```python
import torch
print(torch.cuda.is_available())        # True once the T4 runtime is selected
print(torch.cuda.get_device_name(0))    # Tesla T4
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] The notebook runs top to bottom with no errors after `Runtime > Restart and run all`
- [ ] Every code cell has a markdown note above it explaining what it does
- [ ] Your function returns a value, and you used that value in another cell
- [ ] `torch.cuda.is_available()` printed `True`
- [ ] The notebook is committed to GitHub
- [ ] Your chosen domain and problem are written in a markdown cell

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **It runs** | Clean run from a restarted runtime | Runs with small errors | Does not run |
| **Python basics** | All six items correct and idiomatic | Most correct | Several missing |
| **Notebook quality** | Readable, explained, logically ordered | Readable | Code with no explanation |
| **Domain choice** | Specific problem, clearly stated | Domain named | Not chosen |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [Python tutorial - an informal introduction](https://docs.python.org/3/tutorial/introduction.html)
- [Welcome to Colab](https://colab.research.google.com/notebooks/intro.ipynb)
- [Automate the Boring Stuff (free to read)](https://automatetheboringstuff.com/)
