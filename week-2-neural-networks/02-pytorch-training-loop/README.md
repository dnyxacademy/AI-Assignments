# 2. The Same Network in PyTorch

**Week 2 - Neural networks and deep learning · Tue · Medium · about 3-4 hours**

## What you will practise

- Tensors and autograd
- The standard training loop
- Moving work to the GPU

## Before you start

Finish 01. You need the Colab T4 GPU runtime switched on.

## The task

Rebuild yesterday's network with PyTorch, then train one on your real data.

1. Rebuild the XOR network using `nn.Module`, an optimiser and a loss function. It should be far shorter than your NumPy version - note how much shorter.
2. Identify what `loss.backward()` replaced. You wrote those gradients by hand yesterday; autograd computes them from the forward pass. Write two sentences on what that means.
3. Write the standard training loop and keep it - you will reuse this shape all week:
   `zero_grad` -> forward -> loss -> `backward` -> `step`
4. Now train a network on **your own dataset** from Week 1, predicting the same target as your Week 1 model.
5. Move the model and data to the GPU with `.to(device)`, and time one epoch on CPU against GPU. On a small table the CPU may win - transfer costs something. Say what you observe rather than what you expected.
6. Plot training loss per epoch.
7. Compare the test score with your Week 1 scikit-learn baseline. **If the neural network is worse, say so.** On small tabular datasets it often is, and gradient-boosted trees usually win. Knowing that is worth more than a forced victory.

Step 7 is marked on honesty, not on the score.

## Example

The training loop, which you will write a dozen times:

```python
import torch
import torch.nn as nn

device = 'cuda' if torch.cuda.is_available() else 'cpu'

model = nn.Sequential(
    nn.Linear(2, 3), nn.ReLU(),
    nn.Linear(3, 1), nn.Sigmoid(),
).to(device)

loss_fn   = nn.BCELoss()
optimiser = torch.optim.Adam(model.parameters(), lr=0.01)

for epoch in range(2000):
    optimiser.zero_grad()          # clear last step's gradients, or they accumulate
    prediction = model(X)          # forward
    loss = loss_fn(prediction, y)
    loss.backward()                # autograd fills in every gradient
    optimiser.step()               # apply them
```

Forgetting `zero_grad()` is the classic first PyTorch bug: gradients add up across iterations and training goes strange in a way that is hard to see. Comment it out once, watch what happens, then put it back.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] The XOR network is rebuilt in PyTorch and still solves XOR
- [ ] You wrote what `loss.backward()` replaced from your hand-written version
- [ ] A network is trained on your own dataset
- [ ] Model and data are moved to the GPU, and CPU against GPU is timed
- [ ] A loss curve per epoch is plotted
- [ ] The test score is compared with the Week 1 baseline, honestly, even if worse

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **PyTorch basics** | Idiomatic model, loss and optimiser | Works | Does not train |
| **The training loop** | Correct, including `zero_grad` | Works | Broken or copied blindly |
| **GPU** | Used, and timings reported | Used | Not attempted |
| **Honest comparison** | States the real result and reasons about it | Reports the score | Hides a worse result |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [PyTorch - learn the basics](https://pytorch.org/tutorials/beginner/basics/intro.html)
- [Autograd explained](https://pytorch.org/tutorials/beginner/basics/autogradqs_tutorial.html)
- [Why trees still beat deep learning on tabular data (paper)](https://arxiv.org/abs/2207.08815)
