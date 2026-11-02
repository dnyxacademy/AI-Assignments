# 6. Clustering with k-means

**Week 1 - Python, data and first models · Sat · Easy · about 2 hours**

## What you will practise

- Unsupervised learning: finding structure with no labels
- k-means, and choosing k
- Reading clusters as something meaningful

## Before you start

Finish 05. Self-study lesson - work through it on your own, then bring questions to the catch-up hour.

## The task

Everything so far had a right answer to learn from. This does not.

1. Pick two or three numeric columns from your dataset.
2. Scale them. k-means measures distance, so a column in rupees and a column in years are not comparable until both are scaled - without this, the larger-numbered column decides everything.
3. Run k-means with `k=3`. Plot the points coloured by cluster.
4. Try `k` from 2 to 8, plot the inertia for each, and pick a `k` using the elbow in that curve. Say which `k` you chose and why.
5. **Name your clusters.** Look at the average of each feature within each cluster and give each group a human label - "smallholders with high rainfall", "high-spend repeat customers". If you cannot describe a cluster in words, it may not be a real group.
6. Write three sentences on whether these clusters are useful in your domain, and be willing to conclude that they are not. k-means will always return k clusters, whether or not the data has any.

## Example

Scaling first, then the elbow:

```python
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt

X = StandardScaler().fit_transform(df[['rainfall', 'yield']])

inertias = []
for k in range(2, 9):
    inertias.append(KMeans(n_clusters=k, n_init=10, random_state=42).fit(X).inertia_)

plt.plot(range(2, 9), inertias, marker='o')
plt.xlabel('k'); plt.ylabel('inertia'); plt.title('Elbow plot')
```

Inertia always falls as k rises - with k equal to the number of rows it reaches zero, and means nothing. The elbow is where adding another cluster stops buying much.

Naming the clusters:

```python
df['cluster'] = KMeans(n_clusters=3, n_init=10, random_state=42).fit_predict(X)
df.groupby('cluster')[['rainfall', 'yield']].mean()
# cluster 0: low rainfall, low yield    -> "dry, struggling"
# cluster 1: high rainfall, high yield  -> "well-watered, productive"
# cluster 2: high rainfall, low yield   -> "wet but underperforming" <- the interesting one
```

## Done when

Tick every line yourself before you submit. If one of them fails, the assignment is not finished.

- [ ] Features are scaled before clustering
- [ ] A cluster plot is drawn and coloured by cluster
- [ ] An elbow plot covers k from 2 to 8, and your choice of k is justified
- [ ] Every cluster has a human-readable name based on its feature averages
- [ ] Three sentences assess whether the clusters are actually useful

## How we mark it

| What we look at | Exemplary | Adequate | Needs work |
| --- | --- | --- | --- |
| **Method** | Scaled, correct k-means, reproducible | Runs | Unscaled, or broken |
| **Choosing k** | Elbow used and reasoned | Tried several k | Picked arbitrarily |
| **Interpretation** | Every cluster named and defensible | Some named | Not interpreted |
| **Honesty** | Willing to say the clusters are weak | Partly | Overclaims |

## How to submit

Push the notebook or script to your own repository, open a pull request, then paste the pull request link into the matching lesson in Dnyx Learn. The full steps are in [How to submit](../../README.md#how-to-submit).

## Go deeper (optional, not marked)

- [Clustering (scikit-learn)](https://scikit-learn.org/stable/modules/clustering.html)
- [k-means in depth](https://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_assumptions.html)
- [Visualising k-means (StatQuest, video)](https://www.youtube.com/watch?v=4b5d3muPQmA)
