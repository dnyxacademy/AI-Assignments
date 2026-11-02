# 3. Cleaning, Charts and Data Ethics

**Week 1 - Python, data and first models · Wed · Medium · about 3-4 hours**

## What you will practise

- Handling missing values and outliers, deliberately
- Matplotlib charts that answer a question
- Spotting bias in a dataset before it reaches a model

## Before you start

Finish 02. You need your dataset loaded and understood.

## The task

Clean the data, chart it, and look honestly at what is wrong with it.

1. Deal with missing values. For each column that has them, choose one of: drop the rows, fill with the mean or median, fill with a category like "unknown", or leave them. **Write one sentence per column saying which you chose and why.** There is no universally right answer, and "I dropped everything" is usually the worst one.
2. Find the outliers in one numeric column and decide what they are: a data-entry error, or a real extreme value that belongs. Say which, and what you did.
3. Fix any column whose type was wrong - dates stored as text, numbers stored as text.
4. Draw **five charts**, each answering a question you write above it:
   - a histogram of one numeric column
   - a bar chart of counts per category
   - a scatter plot of two numeric columns
   - a line chart over time, if your data has a date; otherwise a second bar chart comparing groups
   - one chart of your own choosing
   Every chart needs a title and labelled axes. A chart without axis labels says nothing.
5. Write your **findings**: five things you now know about this data that you did not know on Tuesday.
6. **Ethics and bias.** Answer these honestly:
   - who is in this dataset, and who is missing from it?
   - if a model trained on this made decisions about real people, who could it treat unfairly?
   - is there anything in here that should not be used as a feature, even though it would help accuracy?

Section 6 is not a formality. Deciding that a column is legal, available and predictive but should still not be used is part of the job.

## Example

Choosing a fill, and saying why:

```python
# rainfall: 12 missing of 1200. Median, not mean - the column is skewed by a few
# monsoon extremes, and the mean would sit above almost every real value.
df['rainfall'] = df['rainfall'].fillna(df['rainfall'].median())

# district: 40 missing. No sensible value to invent, and the rows are useful
# otherwise, so it becomes a real category.
df['district'] = df['district'].fillna('unknown')
```

A chart that answers a question:

```python
import matplotlib.pyplot as plt

# Question: does yield rise with rainfall, or level off?
plt.scatter(df['rainfall'], df['yield'], alpha=0.5)
plt.title('Yield against rainfall')
plt.xlabel('Rainfall (mm)')
plt.ylabel('Yield (tonnes per hectare)')
plt.show()
```

`alpha=0.5` makes the points translucent, so you can see where many overlap. With solid points, a dense cluster looks like one dot.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] Every column with missing values has a decision and a written reason
- [ ] Outliers in one column are investigated and a judgement recorded
- [ ] Wrong column types are fixed
- [ ] Five charts exist, each with a question above it, a title and labelled axes
- [ ] Five findings are written in plain language
- [ ] All three ethics questions are answered specifically, not generically

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Cleaning decisions** | Each justified and appropriate | Mostly reasonable | Dropped everything, or unexplained |
| **Charts** | All five readable and answer their question | Present, some unlabelled | Missing, or unreadable |
| **Findings** | Specific and genuinely derived from the data | Some real findings | Restates the obvious |
| **Ethics** | Names real groups and a real risk, including a feature to avoid | General answer | Not attempted |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [pandas - working with missing data](https://pandas.pydata.org/docs/user_guide/missing_data.html)
- [Matplotlib - pyplot tutorial](https://matplotlib.org/stable/tutorials/pyplot.html)
- [Google - fairness in machine learning](https://developers.google.com/machine-learning/crash-course/fairness/types-of-bias)
