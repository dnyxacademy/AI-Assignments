# 2. NumPy, pandas and Your Dataset

**Week 1 - Python, data and first models · Tue · Easy · about 3 hours**

## What you will practise

- NumPy arrays and vectorised operations
- pandas DataFrames: loading, selecting, filtering
- Reading a dataset you have never seen before

## Before you start

Finish 01. You need a working Colab notebook and your chosen domain.

## The task

Find a real dataset in your domain and get to know it.

1. Find a dataset. Good free sources: [Kaggle Datasets](https://www.kaggle.com/datasets), [data.gov.in](https://data.gov.in/), [UCI](https://archive.ics.uci.edu/datasets), [HuggingFace Datasets](https://huggingface.co/datasets). It must have **at least 500 rows and 5 columns**, and should be about your domain.
2. Load it with `pandas.read_csv()` into a DataFrame.
3. Answer these in the notebook, each with the code that found the answer:
   - how many rows and columns?
   - what is the type of each column, and does any type look wrong?
   - how many missing values are in each column?
   - for each numeric column: minimum, maximum, mean, median
   - for one text column: how many distinct values, and the five commonest
4. Select and filter: one column on its own, two columns together, and the rows matching a condition that means something in your domain.
5. Use NumPy on one numeric column to do the same sum with a loop and then vectorised, and time both with `%timeit`. Write down the difference.
6. In a markdown cell, write five sentences on what this dataset actually contains, as if explaining it to someone who has never seen it.

Step 6 matters more than it looks. Models built on data nobody understood are the commonest way an AI project quietly fails.

## Example

A first look, in four lines:

```python
import pandas as pd

df = pd.read_csv('crops.csv')
print(df.shape)            # (1200, 8)  -> rows, columns
print(df.dtypes)           # types per column: watch for numbers read as object
print(df.isna().sum())     # missing values per column
df.describe()              # min, max, mean, quartiles for numeric columns
```

Why vectorising matters:

```python
import numpy as np
values = df['yield'].to_numpy()

%timeit sum(v for v in values)   # Python loop, element by element
%timeit values.sum()             # NumPy, the whole array at once - usually 50x faster or more
```

NumPy is faster because the loop runs in compiled code over one contiguous block of memory, instead of in Python one object at a time.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] The dataset has at least 500 rows and 5 columns, and relates to your domain
- [ ] Its source is linked in the notebook
- [ ] All five questions in step 3 are answered with code, not guessed
- [ ] Selecting, filtering and a domain-meaningful condition all work
- [ ] The loop and vectorised timings are both recorded, with the difference noted
- [ ] Five sentences describe what the data contains, in plain language

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Dataset choice** | Real, relevant, big enough, source cited | Acceptable | Toy dataset, or no source |
| **Exploration** | All questions answered with correct code | Most answered | Superficial |
| **pandas fluency** | Selecting and filtering are idiomatic | Works | Struggles with basics |
| **Understanding** | Describes the data accurately and plainly | Partly | Cannot say what it holds |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [pandas - 10 minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html)
- [NumPy - the absolute basics for beginners](https://numpy.org/doc/stable/user/absolute_beginners.html)
- [Kaggle Datasets](https://www.kaggle.com/datasets)
