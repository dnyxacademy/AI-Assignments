# 4. Your First Model

**Week 1 - Python, data and first models · Thu · Medium · about 3-4 hours**

## What you will practise

- Features and labels, training and test sets
- Linear regression, logistic regression, decision trees
- Overfitting, and why you never test on training data

## Before you start

Finish 03. You need a clean dataset.

## The task

Train a model that predicts something, and understand why it is only a baseline.

1. Decide what you are predicting. Pick a target column, and say whether this is **regression** (a number, like yield or price) or **classification** (a category, like approved or rejected).
2. Choose your features - the columns the model may look at. Exclude anything that leaks the answer. If you are predicting whether a loan defaulted, a column called `amount_recovered` is leakage, not a feature.
3. Split the data with `train_test_split`, holding back 20 percent for testing, with `random_state` set so the split is reproducible.
4. Train a simple model: linear regression for a number, logistic regression or a decision tree for a category.
5. Score it on **both** the training set and the test set.
6. Compare the two scores and say what the gap means. A model scoring 0.99 on training and 0.61 on test has memorised the training rows instead of learning the pattern - that is overfitting, and the single most important idea this week.
7. Train a deliberately overfitted model to see it happen: a decision tree with no depth limit. Record both scores again.
8. Write down a **baseline to beat**: what score would you get by always predicting the mean, or the commonest class? If your model cannot beat that, it has learned nothing.

Step 8 is the one people skip and regret. A classifier reporting 94 percent accuracy sounds excellent, until you notice 94 percent of the rows are one class and predicting it every time scores the same.

## Example

The split, the fit, and both scores:

```python
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeRegressor

X = df[['rainfall', 'temperature', 'fertiliser']]   # features
y = df['yield']                                      # the target

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42,
)

model = DecisionTreeRegressor(max_depth=4, random_state=42).fit(X_train, y_train)

print('train', model.score(X_train, y_train))   # 0.78
print('test ', model.score(X_test, y_test))     # 0.71  <- a small gap is healthy
```

Now with no limit, so you can watch it overfit:

```python
greedy = DecisionTreeRegressor(random_state=42).fit(X_train, y_train)
print('train', greedy.score(X_train, y_train))  # 1.00  <- memorised every row
print('test ', greedy.score(X_test, y_test))    # 0.52  <- worse on unseen data
```

The dumb baseline:

```python
from sklearn.dummy import DummyRegressor
dumb = DummyRegressor(strategy='mean').fit(X_train, y_train)
print('baseline', dumb.score(X_test, y_test))   # 0.00 -- beat this, or you have nothing
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] The target is chosen, and named as regression or classification
- [ ] Features are listed, and leakage columns are excluded on purpose
- [ ] The split uses a fixed `random_state`
- [ ] A model is trained and scored on both training and test sets
- [ ] The gap between the two is explained in your own words
- [ ] The deliberately overfitted model is trained and both scores recorded
- [ ] A dumb baseline score is computed, and your model beats it

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Problem framing** | Target and task type correct, leakage avoided | Mostly right | Leakage, or task misidentified |
| **Training** | Correct split and fit, reproducible | Works | Tests on training data |
| **Understanding overfitting** | Explains the gap and demonstrates it | Mentions it | Not understood |
| **Baseline** | Computed, compared, and beaten | Computed | Not considered |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [scikit-learn - an introduction to machine learning](https://scikit-learn.org/stable/tutorial/basic/tutorial.html)
- [Underfitting and overfitting (scikit-learn)](https://scikit-learn.org/stable/auto_examples/model_selection/plot_underfitting_overfitting.html)
- [Google ML crash course - generalisation](https://developers.google.com/machine-learning/crash-course/overfitting/generalization)
