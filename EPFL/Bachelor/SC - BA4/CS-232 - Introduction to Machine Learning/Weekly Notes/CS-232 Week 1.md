---
Week: 1
Themes:
  - Basic Concepts
  - Linear Regression
aliases: []
Lecture1: true
pExercises: true
Exercises: true
---

# Notes

## Lecture 1

### Forewords
- Machine learning is great but it is not sufficient to pass interviews in big US tech
- You need to specifically prepare for coding interviews
- Ressources:
	- [Main book](https://github.com/Avinash987/Coding/blob/master/Cracking-the-Coding-Interview-6th-Edition-189-Programming-Questions-and-Solutions.pdf)
	- [alternate1](https://github.com/Mcdonoughd/CS2223/blob/master/Books/Algorithhms%204th%20Edition%20by%20Robert%20Sedgewick%2C%20Kevin%20Wayne.pdf)
	- [alt 2](https://sd.blackball.lv/library/Introduction_to_Algorithms_Third_Edition_(2009).pdf)
	- [[Get Prepped for Tech PPT Deck 9-29-22_withTechExample.pdf]]
#### What is Machine Learning:
An algorithm that learns from experience.
Three components:
- Data: Many different sources of data (image, text, sound, mixed), Supervised vs Unsupervised
- Algorithms: 
	- Supervised learning: relies on supervised data, labels correspond to desired insight
		- Train: Use data and ground proofs to optimise model parameters
		- Test: Predict output for data unseen by model during training
	- Unsupervised learning: relies on unsupervised data, goal is to analyse observed data set
		- Classify data into a number of groups
	- Reinforcement learning (not covered): learn to react to environment
- Insights: precise concrete prediction (classification, value, behavior, etc...)
#### Focus
In this class focus on two main classes of algorithms: Regression, Classification

**Regression**: prediction one or multiple continuous value(s)
**Classification**: classifying input into different categories, prediction label for a given sample

#### Data

Training set and Test set should always be completely separate but draw from the same statistical distribution

### The Linear Model

 $x_i \in \mathbb{R}^D$ : $i^{th}$ data sample in the collection N, $x_i$ is a column vector of dim D 
 $y_i$ is the $i^{th}$ output --> Category for classification, value vector of dim C for regression

Simple parametric model defined by 2 parameters: 
- y-intercept $w^{(0)}$
- slope $w^{(1)} = \frac{\Delta y}{\Delta x}$
- Gives line: $y = w^{(0)} + w^{(1)}x$

#### Training
Find best $w^{(0)^*}$ and $w^{(1)^*}$ for some given data

Given N training pairs $\{(x_i, y_i)\}$, we aim to find $w^{(0)}$ and $y^{(0)}$ such that the prediction values $\hat{y_i} = w^{(0)} + w^{(1)}x$ are close to $y_i$

#### Measure of closeness

Measure of error is Euclidian distance (L2 norm): (see slides)
In practice more common to use squared Euclidian distance: $d^{2}(\hat{y}_{i}, y_{i})= (\hat{y}_{i} - y_{i})^{2}$

