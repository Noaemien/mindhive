---
Week: 2
Themes: 
aliases: 
Lecture1: true
pExercises: false
Exercises: true
---

# Notes

## Lecture 1

Ressource: [Matrix Cookbook](http://www2.imm.dtu.dk/pubdb/edoc/imm3274.pdf)

Training a linear regressor aims to minimise: $R(w^{(0)}, w^{(1)}) = \frac{1}{N} \sum^{N}_{i = 1}d^{2}(\hat{y}_{i}, y_{i})$

In dimension $D$ we can write: $y = w^{(0)} + w^{(1)}x^{(1)} + \dots + w^{(D)}x^{(D)} = \boldsymbol{w^{T}}\begin{bmatrix}x^{(0)} \\ x^{(1)} \\ \vdots \\ x^{(D)}\end{bmatrix}$

Gradient of $R(w)$ is $\nabla_{w} R(w) = \begin{bmatrix}\frac{\delta R}{\delta w^{(1)}} \\ \frac{\delta R}{\delta w^{(2)}}  \\ \vdots \\ \frac{\delta R}{\delta w^{(D)}} \end{bmatrix}$

### Solution
To obtain the solution we search for $w^{*}$ such that $$\nabla_{w} R(w) = \frac{2}{N}\sum^{N}_{i=1}\boldsymbol{x_{i}}(\boldsymbol{x_{i}^{T}}\boldsymbol{w^{*}} - y_{i}) = 0$$
$$\implies \left(\sum^{N}_{i=1} \boldsymbol{x_{i} x_{i}^{T}}\right)\boldsymbol{w^{*}} = \sum^{N}_{i = 1} \boldsymbol{x_{i}}y_{i}$$
$$\implies \boldsymbol{X^{T}X w^{*}} = \boldsymbol{X^{T}y}$$

### Evaluation Metrics for Regression

(slide 42 - 43)

### Multiple output dimensions

$\boldsymbol{w}$ becomes a matrix $\boldsymbol{W}$ so that the product $\boldsymbol{W^{T}x_{i}}$ yields a vector

we get:  $\hat{y_{i}} = \boldsymbol{W^{T}x_{i}} = \begin{bmatrix}w^{T}_{(1)} \\ w^{T}_{(2)} \\ \vdots \\ w^{T}_{(D)}  \end{bmatrix} \boldsymbol{x_{i}}$ 