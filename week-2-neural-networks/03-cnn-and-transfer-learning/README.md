# 3. Convolutional Networks and Transfer Learning

**Week 2 - Neural networks and deep learning · Wed · Medium · about 4 hours**

## What you will practise

- Convolution, and why it suits images
- Transfer learning from a pretrained model
- Freezing layers, and fine-tuning

## Before you start

Finish 02. GPU runtime on. Workshop day - come with your dataset chosen.

## The task

Train an image classifier without the data or the compute you would normally need, by starting from a model someone else trained.

1. Pick a small image dataset - two to five classes, at least 100 images per class. Use a public one, or collect your own and say how.
2. Train a **small CNN from scratch** first: two or three convolutional layers, then pooling, then a linear head. Record its accuracy. It will probably be mediocre, which is the point.
3. Now load a **pretrained model** (ResNet18 or MobileNetV2, pretrained on ImageNet), freeze the convolutional layers, replace the final layer with one sized for your classes, and train only that. Record the accuracy.
4. Compare. Transfer learning usually wins by a wide margin on a small dataset. Explain **why** in your own words: the early layers already detect edges, textures and shapes, and those are useful for nearly any image task.
5. Unfreeze the last block and fine-tune with a much smaller learning rate. Record whether it helped.
6. Show the confusion matrix and look at **five images it got wrong**. Say what they have in common - that tells you more about your data than any accuracy number.
7. Apply the same idea to text if your domain suits it better than images: a pretrained sentence model, with a classifier on top.

Step 6 is where the real learning is. Misclassified examples usually reveal a problem with the data rather than the model.

## Example

Freezing, then replacing the head:

```python
import torch.nn as nn
from torchvision import models

model = models.resnet18(weights=models.ResNet18_Weights.DEFAULT)

for param in model.parameters():
    param.requires_grad = False          # freeze everything it already learned

model.fc = nn.Linear(model.fc.in_features, num_classes)   # new, trainable head

# only the new layer's parameters go to the optimiser
optimiser = torch.optim.Adam(model.fc.parameters(), lr=1e-3)
```

Then fine-tuning, carefully:

```python
for param in model.layer4.parameters():
    param.requires_grad = True

# 1e-4, not 1e-3: a large step here would wreck features that took
# thousands of GPU-hours to learn
optimiser = torch.optim.Adam(
    [{'params': model.layer4.parameters(), 'lr': 1e-4},
     {'params': model.fc.parameters(),     'lr': 1e-3}]
)
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] A dataset with at least two classes and 100 images per class, source stated
- [ ] A from-scratch CNN is trained and its accuracy recorded
- [ ] A pretrained model with a replaced head is trained and recorded
- [ ] The two are compared, with an explanation of why transfer learning wins
- [ ] Fine-tuning with a smaller learning rate is tried and the result recorded
- [ ] A confusion matrix is shown
- [ ] Five misclassified images are examined and what they share is written down

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Both models** | Scratch and pretrained both trained and compared | One trained | Neither works |
| **Transfer learning** | Correct freezing and head replacement | Works | Retrains everything by accident |
| **Error analysis** | Five errors examined with a real insight | Confusion matrix only | Not attempted |
| **Explanation** | Explains why pretrained features transfer | Roughly right | Missing |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [Transfer learning tutorial (PyTorch)](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)
- [CS231n - convolutional networks](https://cs231n.github.io/convolutional-networks/)
- [torchvision models](https://pytorch.org/vision/stable/models.html)
