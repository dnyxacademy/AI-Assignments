# 1. A Tiny Network, by Hand

**Week 2 - Neural networks and deep learning · Mon · Hard · about 4 hours**

## What you will practise

- The neuron: weights, bias, activation
- Loss, gradient descent, and backpropagation
- Building it in NumPy before any framework

## Before you start

Finish Week 1. You need NumPy and comfort with functions.

## The task

Before PyTorch does it for you, do it yourself once. This is the assignment that makes deep learning stop feeling like magic.

1. Implement one neuron in NumPy: it takes inputs, multiplies by weights, adds a bias, and passes the result through a sigmoid.
2. Build a network with one hidden layer of two or three neurons, and one output.
3. Write the **forward pass**: inputs in, prediction out.
4. Write a **loss function**: mean squared error between prediction and truth.
5. Write the **backward pass** by hand - the gradient of the loss with respect to each weight - and update the weights by a small step in the opposite direction. This is gradient descent.
6. Train it on something tiny with a known answer: XOR is traditional, four rows, and cannot be solved without a hidden layer. Loop a few thousand times and print the loss every few hundred.
7. Plot the loss curve. It should fall. If it is flat, your learning rate is too small or a gradient is wrong; if it swings wildly, the learning rate is too large.
8. Write five sentences explaining backpropagation in plain language, as if to someone who knows no calculus.

Expect this to take the full session and to be frustrating. That is the assignment working. You only have to do it once, and every framework afterwards makes sense.

## Example

Forward, then one step back:

```python
import numpy as np

def sigmoid(x):  return 1 / (1 + np.exp(-x))
def dsigmoid(y): return y * (1 - y)        # derivative, given the output

X = np.array([[0,0],[0,1],[1,0],[1,1]])
y = np.array([[0],[1],[1],[0]])            # XOR

rng = np.random.default_rng(42)
W1, b1 = rng.normal(size=(2, 3)), np.zeros((1, 3))
W2, b2 = rng.normal(size=(3, 1)), np.zeros((1, 1))
lr = 0.5

for step in range(10000):
    h   = sigmoid(X @ W1 + b1)             # hidden layer
    out = sigmoid(h @ W2 + b2)             # prediction

    loss = np.mean((out - y) ** 2)

    d_out = (out - y) * dsigmoid(out)      # how wrong, and which way
    d_h   = (d_out @ W2.T) * dsigmoid(h)   # push the blame back a layer

    W2 -= lr * h.T @ d_out                 # step downhill
    b2 -= lr * d_out.sum(axis=0, keepdims=True)
    W1 -= lr * X.T @ d_h
    b1 -= lr * d_h.sum(axis=0, keepdims=True)

    if step % 2000 == 0:
        print(step, round(loss, 4))
```

`d_h = (d_out @ W2.T) * dsigmoid(h)` is backpropagation in one line: take the error at the output, send it back through the weights that produced it, and scale it by how sensitive that neuron was. Nothing more mysterious than that.

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] A single neuron works: weights, bias and activation
- [ ] The network has at least one hidden layer
- [ ] Forward pass, loss and backward pass are all written by you, with no framework
- [ ] It learns XOR - predictions round to the right four answers
- [ ] A loss curve is plotted and falls
- [ ] Backpropagation is explained in five plain sentences, without calculus notation

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Implementation** | Forward and backward both correct from scratch | Forward correct, backward shaky | Does not work |
| **It learns** | XOR solved, loss near zero | Loss falls but does not solve it | Loss flat |
| **Loss curve** | Plotted and discussed | Plotted | Missing |
| **Explanation** | Genuinely clear to a non-mathematician | Roughly right | Copied jargon |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [3Blue1Brown - neural networks (video series)](https://www.3blue1brown.com/topics/neural-networks)
- [A neural network from scratch in Python](https://victorzhou.com/blog/intro-to-neural-networks/)
- [Backpropagation calculus (3Blue1Brown)](https://www.youtube.com/watch?v=tIeHLnjs5U8)
