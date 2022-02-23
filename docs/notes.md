---
layout: default
description: ML Notes
title: Quick Notes
permalink: /notes/
---

## Assumptions of linear regression?

- Observations are independent
- Linear relationship (ofc)
- Residuals follow a normal dist.
- No multi-correlation b/w features

[Source](https://towardsdatascience.com/assumptions-of-linear-regression-fdb71ebeaa8b)

----

## How to find K in K-means clustering?

- Elbow Method (try various values of k)
- Silhoutte Method

[Source](https://medium.com/analytics-vidhya/how-to-determine-the-optimal-k-for-k-means-708505d204eb)

----

## Accuracy of clustering algorithm?

### Precision
- For each cluster, find class with max number in it.
- Sum this over all clusters and divide by total objects.

### Recall
- For each class, find cluster having max number of it, store this num.
- Sum this number over all classes and divide by total objects.

### F1 Score
- Harmonic mean of Precision and Recall

### Adjusted Rand Index
- Measure of how similar the clustering is overall

[Source](https://towardsdatascience.com/evaluating-clustering-results-f13552ee7603)

----

## Regularization for Trees

- Limit max depth of the tree
- Ensembles/Bagging more than 1 tree
- Strict stopping criterion on splitting node further

[Source](https://www.quora.com/How-is-regularization-performed-on-simple-decision-trees/answer/Satendra-Kumar?ch=10&oid=63449982&share=23cde347&srid=oQch&target_type=answer)

----

## Bias-Variance

- Bias = $$(E(\text{predicted value}) - \text{true value})^2$$
- Variance = $$E(\text{predicted value} - avg.(\text{all predicted values}))$$
- High Train Error : High Bias
- High Test Error : High Variance

[Source](https://towardsdatascience.com/bias-variance-tradeoff-in-machine-learning-models-a-practical-example-cf02fb95b15d)

----

## Gradient Descent

- First order optimisation algorithm to find local minima

Requirements:
- Differentiable
- Convex or Quasi-Convex

[Source](https://towardsdatascience.com/gradient-descent-algorithm-a-deep-dive-cf04e8115f21)

----

## Handle Categorical and Missing Input Feature Data

- Assign numbers/indexing to each category
- Create a 1/0 binary feature vector for each category
- Replace category with a mean of continuous feature in that category
- For missing data, perform imputation with mean/median of that category

[Source](https://towardsdatascience.com/feature-handling-3f14c12ecbb8)

----

## Handle Data Imbalance for Binary Classification

- Sampling Methods (Act on the data)
	- Oversampling/Undersampling
	- SMOTE (Synthetic Minority Oversampling TEchnique)
- Cost-sensitive methods (Act on the cost function)

[Source](https://towardsdatascience.com/guide-to-classification-on-imbalanced-datasets-d6653aa5fa23)

----

## Gradient Clipping

- To solve exploding gradients problem
- if $$\vert\vert g \vert\vert > c$$ , rescale it so that norm is exactly $$c$$
- $$c$$ is a hyperparameter

[Source](https://towardsdatascience.com/what-is-gradient-clipping-b8e815cdfb48)

----

## HDBSCAN

- Define density of a point as inverse of the distance to it's Kth nearest neigbhour
- Plot density vs the points in its feature space, mountains could be clusters
- Select different thresholds to find mountains based on some criteria

[Source](https://towardsdatascience.com/a-gentle-introduction-to-hdbscan-and-density-based-clustering-5fd79329c1e8)

----

## Dimensionality Reduction

- LSI: Split into SVD as $$X = USV^T$$, pick top $$k$$ right singular vectors i.e. $$V_k, X_red = X * V_k$$
- PCA: Make unit var and 0 mean, take COV matrix, perform EVD, pick $$k$$ largest eig vectors as $$V_k$$ (same as above)
- NMF: Find $$W,H$$ to min $$\vert\vert X-WH \vert\vert$$, use $$W$$ as dimensionality reduced version of $$X$$

[Source](https://stats.stackexchange.com/questions/134282/relationship-between-svd-and-pca-how-to-use-svd-to-perform-pca)

----