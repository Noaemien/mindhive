---
Week: 5
Themes:
  - k-Nearest Neighbors
  - Feature Expansion
aliases: 
Lecture1: true
pExercises: false
Exercises: false
---

# The simplest nonlinear ML Algo: Nearest neighbor method

## Algorithm

1. Compute the distance between the test sample $x$ and all training samples $\{x_i\}$
2. Find the sample $x_{NN}$ with the minimum distance
3. Assign the corresponding label/value $y_{NN}$ to the test sample

## Problem
Sensitive to outliers

# Histograms
A histogram in $\R^{D}$ is a D-dimensional vector such that $$x^{(d)}\in [0,1], \forall1\le d \le D$$ $$\sum\limits^{D}_{d=1}x^{(d)}= 1 $$
# k-Nearest Neighbors

## Classification
1. Compute the distance between the test sample $x$ and all training samples $\{x_i\}$ 
2. Find the $k$ samples $\{x_{NN1}, \dots, x_{NNk}\}$ with min distances
3. Find the most common label among these k nearest neighbors (majority vote)
4. Assign the corresponding label $y_{MV}$ to the test sample

- If several labels appear the same number of times among the k-NN, one strategy consists of taking the one whose samples have the smallest average distance to the test sample
## Properties
1. No learning, only need
	- Good data representation
	- distance function
	- value $k$
2. Non parametric learning method
	- No parameters to optimize
	- $k$ is a hyper-parameter
3. Computationally expensive