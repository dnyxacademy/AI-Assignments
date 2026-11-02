# 5. Overfitting, Regularisation and Tracking Experiments

**Week 2 - Neural networks and deep learning · Fri · Medium · about 3 hours**

## What you will practise

- Dropout, weight decay, early stopping, augmentation
- Reading training curves
- Keeping a record so results are reproducible

## Before you start

Finish 04. You need a model that trains.

## The task

Make a model overfit on purpose, then fix it four different ways, and keep a record of what each did.

1. Train until it clearly overfits: training loss keeps falling while validation loss turns upward. Plot both on one chart and mark where they diverge. **That crossing point is what overfitting looks like** - learn to recognise it on sight.
2. Now apply four techniques, one at a time, recording validation score for each:
   - **dropout** - randomly ignore some neurons each step
   - **weight decay** - penalise large weights
   - **early stopping** - stop when validation stops improving
   - **data augmentation** - for images, flips, crops and rotations; for tabular data, more data or noise
3. Build a results table: technique, setting, training score, validation score, what you concluded.
4. Combine the two that helped most. Note whether the combination beat either alone - sometimes it does not.
5. Set every random seed and confirm two runs give the same numbers. An unreproducible result cannot be reported.
6. Write a short `EXPERIMENTS.md`: what you tried, what worked, what did not, and what you would try next with more time.

Keeping the record is the actual professional skill here. Six months on, an untracked experiment is a result you cannot defend or repeat.

## Example

The divergence you are looking for:

```
epoch  train_loss  val_loss
  1      0.69       0.68
  5      0.41       0.44
 10      0.22       0.39        <- val_loss has stopped improving
 15      0.09       0.47        <- and is now rising: memorising, not learning
 20      0.03       0.58        <- early stopping should have fired at ~epoch 10
```

Dropout, and seeds for reproducibility:

```python
model = nn.Sequential(
    nn.Linear(n_features, 64), nn.ReLU(),
    nn.Dropout(0.3),                       # ignore 30 percent of neurons each step
    nn.Linear(64, 1),
)

optimiser = torch.optim.Adam(model.parameters(), lr=1e-3, weight_decay=1e-4)
```

```python
import random, numpy as np, torch
SEED = 42
random.seed(SEED); np.random.seed(SEED)
torch.manual_seed(SEED); torch.cuda.manual_seed_all(SEED)
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] A chart shows training and validation loss diverging, with the point marked
- [ ] All four techniques are tried separately and scored
- [ ] A results table records setting, both scores and a conclusion per technique
- [ ] The best two are combined and the outcome noted
- [ ] Seeds are set and two runs produce identical numbers
- [ ] `EXPERIMENTS.md` is written and committed

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Demonstrating overfitting** | Clear divergence, correctly identified | Shown | Not demonstrated |
| **Regularisation** | All four tried and measured | Two or three | One, or none |
| **Record keeping** | Table and notes let someone repeat it | Some notes | Nothing recorded |
| **Reproducibility** | Seeds set, identical reruns confirmed | Seeds set | Not attempted |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [Dropout, the original paper](https://jmlr.org/papers/v15/srivastava14a.html)
- [PyTorch - overfitting and regularisation](https://pytorch.org/tutorials/beginner/basics/optimization_tutorial.html)
- [Google ML crash course - regularisation](https://developers.google.com/machine-learning/crash-course/overfitting/regularization)
