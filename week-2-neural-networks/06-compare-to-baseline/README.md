# 6. Deep Learning Against Your Week 1 Baseline

**Week 2 - Neural networks and deep learning · Sat · Medium · about 2-3 hours**

## What you will practise

- Comparing models fairly
- Deciding whether complexity earned its keep
- Writing up a result honestly

## Before you start

Finish 01 to 05, and have your Week 1 model to hand. This is the week 2 submission.

## The task

Put the two approaches side by side and reach a defensible conclusion.

1. Evaluate your Week 1 scikit-learn model and your Week 2 neural network **on exactly the same test set**, with the same metrics. A comparison across different splits is not a comparison.
2. Add a third contender: gradient boosting (`sklearn.ensemble.HistGradientBoostingClassifier` or `XGBoost`). On tabular data it frequently beats both.
3. Build the comparison table: model, metrics, training time, number of parameters or rough size, and how long it took **you** to build.
4. Pick a winner, and justify it on more than score. A model two percent better that takes twenty times longer to train and cannot be explained to a stakeholder may still be the wrong choice.
5. Answer directly: **was the neural network worth it for this problem?** For small tabular datasets the honest answer is usually no, and saying so is a correct, well-marked answer.
6. Write it up in `RESULTS.md` with your charts, the table, and your conclusion.

This assignment is marked on the quality of your reasoning, not on which model won.

## Example

A comparison table worth reading:

```markdown
| Model | Test F1 | Train time | Size | My time to build |
| --- | --- | --- | --- | --- |
| Logistic regression (wk 1) | 0.71 | 0.2 s | 9 params | 20 min |
| Neural net, 2 layers (wk 2) | 0.74 | 95 s (GPU) | 4,289 params | 4 h |
| HistGradientBoosting | **0.79** | 1.8 s | ~100 trees | 15 min |

**Conclusion.** Gradient boosting wins on every axis that matters here: best F1,
trains in under two seconds on a CPU, and took me fifteen minutes. The neural
network beat logistic regression by 0.03 F1 and cost four hours and a GPU.

With 1,200 rows and 8 tabular features there is not enough data for a network
to find structure the trees miss. I would reach for a network here only if the
data grew past ~100k rows, or if I needed to mix in text or images, which trees
handle poorly.
```

Note that this write-up concludes against the thing the week was about, and is stronger for it.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] All models are evaluated on the identical test set with identical metrics
- [ ] A gradient boosting model is included
- [ ] The table records scores, training time, size and your build time
- [ ] A winner is chosen and justified beyond its score
- [ ] The question "was the neural network worth it?" is answered directly
- [ ] `RESULTS.md` is committed with charts, table and conclusion

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Fair comparison** | Same split and metrics throughout | Mostly fair | Not comparable |
| **Completeness** | All three models, full table | Two models | One model |
| **Reasoning** | Weighs cost, speed and explainability, not just score | Some reasoning | Picks the highest number |
| **Honesty** | Willing to conclude the network was not worth it | Partly | Overclaims |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [HistGradientBoosting (scikit-learn)](https://scikit-learn.org/stable/modules/ensemble.html#histogram-based-gradient-boosting)
- [XGBoost introduction](https://xgboost.readthedocs.io/en/stable/tutorials/model.html)
- [Tabular data: deep learning is not all you need](https://arxiv.org/abs/2106.03253)
