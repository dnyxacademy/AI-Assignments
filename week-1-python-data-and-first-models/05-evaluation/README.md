# 5. Evaluation: Beyond Accuracy

**Week 1 - Python, data and first models · Fri · Medium · about 3 hours**

## What you will practise

- Accuracy, precision, recall, F1, and the confusion matrix
- Cross-validation
- scikit-learn pipelines

## Before you start

Finish 04. You need a trained model.

## The task

Measure your model properly. Accuracy on its own hides most of what goes wrong.

1. Build a **confusion matrix** for a classification model and read it out loud: how many true positives, false positives, true negatives, false negatives. If your task is regression, also train a simple classifier on some threshold of your target, so you can do this part.
2. Compute precision, recall and F1. Then answer, **for your domain specifically**: which is worse here, a false positive or a false negative? There is a real answer, and it depends on what the model is used for. A model that flags a disease should not miss cases; one that flags fraud should not accuse honest customers too often.
3. Use **cross-validation** (`cross_val_score`, 5 folds) instead of a single split, and compare. If the fold scores vary a lot, a single split was partly luck.
4. Build a **pipeline** that puts your preprocessing and your model in one object, so the same steps apply to training and test data in the same order. Doing preprocessing by hand on both sets is how data leaks between them.
5. Produce one **evaluation table** comparing at least three models on the same metrics.
6. Write three sentences recommending one model, with reasons that include its weaknesses, not only its score.

## Example

Reading a confusion matrix:

```python
from sklearn.metrics import confusion_matrix, classification_report

print(confusion_matrix(y_test, predictions))
# [[80  5]      80 correctly said "no",  5 wrongly said "yes"  (false positives)
#  [12 23]]     12 wrongly said "no",   23 correctly said "yes" (false negatives = 12)

print(classification_report(y_test, predictions))
```

Those 12 false negatives are the number that matters if this model screens for a disease: twelve ill people were sent home. Accuracy here is 86 percent, which sounds fine and is not.

A pipeline, so preprocessing cannot leak:

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import cross_val_score

pipe = Pipeline([
    ('scale', StandardScaler()),
    ('model', LogisticRegression(max_iter=1000)),
])

scores = cross_val_score(pipe, X, y, cv=5)
print(scores, scores.mean().round(3), scores.std().round(3))
```

The scaler is fitted inside each fold, on that fold's training part only. Scaling the whole dataset first would let test-set information reach the model.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] A confusion matrix is produced and all four cells are explained in words
- [ ] Precision, recall and F1 are computed
- [ ] You argued which of a false positive or false negative is worse in your domain, and why
- [ ] 5-fold cross-validation is run and compared with the single split
- [ ] A pipeline holds preprocessing and model together
- [ ] An evaluation table compares at least three models
- [ ] Your recommendation names a weakness, not only a score

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Metrics** | All computed and correctly interpreted | Computed | Accuracy only |
| **Domain reasoning** | A clear, correct argument about which error costs more | Some reasoning | Not attempted |
| **Cross-validation** | Run, and the variance discussed | Run | Not used |
| **Pipeline** | Correct, prevents leakage | Built | Manual preprocessing on both sets |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [scikit-learn - metrics and scoring](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [Cross-validation (scikit-learn)](https://scikit-learn.org/stable/modules/cross_validation.html)
- [Precision and recall (Google ML crash course)](https://developers.google.com/machine-learning/crash-course/classification/accuracy-precision-recall)
