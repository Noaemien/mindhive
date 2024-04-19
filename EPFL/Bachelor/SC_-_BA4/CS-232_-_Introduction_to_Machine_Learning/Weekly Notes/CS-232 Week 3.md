---
Week: 3
Themes:
  - Linear Regression
  - Classification
aliases: 
Lecture1: true
pExercises: false
Exercises: true
---

# Notes

## Lecture 1

### Adding Non linearity

Gives the ability to better model classification problems than linear models
#### Step function

0 derivative for *almost* all points 


#### Logistic sigmoid function

$$\sigma(a) = \frac{1}{1 + exp(-a)}$$

```functionplot
---
title: Sigmoid
xLabel: 
yLabel: 
bounds: [-10,10,-0.1,1.1]
disableZoom: true
grid: false
---
Sigmoid(x) = 1 / (1 + exp(-x))
```



### Logistic regression

The model who's prediction is : $\hat{y} = \sigma(w^{T}x) = \frac{1}{1 + exp(-w^{T}x)}$ is called *logistic regression* 
It is a classification model.

#### Training

We use the *cross entropy* loss function (slide 37)

##### Cross entropy
Convex function that we optimise using gradient descent
Log likelihood of predictions with respect to real values.
Likelyhood: $p(y | w) = \prod^{N}_{i = 1} \hat{y}^{y_{i}} * (1-\hat{y})^{1 - y_{i}}$ 
Empirical Risk = $$R(w) = \sum\limits^{N}_{i=1} (y_{i}\ln(\hat{y}_i) + (1-y_{i})\ln(1-\hat{y_{i}}))$$

### Support Vector Machines (SVM)
**Objective**: maximise margin between decision boundary and training samples
Boundary is determined by a subset of datapoints called support vectors

SVM classifier is a binary classifier based on linear model as before: $\hat{y} = w^{T}x$ 
