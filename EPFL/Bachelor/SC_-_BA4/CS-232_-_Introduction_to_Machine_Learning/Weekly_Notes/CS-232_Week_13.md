---
Week: 13
Themes: 
aliases: 
Lecture1: true
pExercises: false
Exercises: false
---

# Clustering
Identifying groups without performing any data transformations.
Example: MNIST, clustering numbers 

## k-means clustering
- Given a set of input samples, group the samples into k clusters
- K is a hyper-parameter, assumed to be known, given

**Intuition**
- The distances between points within a cluster should be small
- Distances across clusters should be large
![[Pasted image 20240521083730.png|200]]

### Algorithm
1. Assign each point $x_{i}$ to the nearest center $\mu_{k}$ 
	- For each point $x_{i}$, compute the Euclidian distance to every center {$\mu_{1}$, $\dots$, $\mu_{k}$}
	- Find the smallest distance
	- The point is said to be assigned to the corresponding cluster (note that each point is assigned to a single cluster)
2. Update each center $\mu_{k}$ based on points assigned to it
	- Recompute each center $\mu_{k}$ as the mean of the points that were assigned to it
3. While not converged
	- The algo iteratively updates the cluster assignment for each point and the cluster center. We need to eventually stop iterating
	- This could be achieved by a fixed number of iterations
	- The algo is guaranteed to converge to a stable solution, where the cluster centers and the point assignments are fixed 
	- The difference in assignements or center locations between two iterations can be used as criteria to stop the algorithm

### Properties
✔ Simple algorithm guaranteed to converge
🚫 Does not always converge to best solution
-> Run algorithm several times with different initialization
- For each solution, sum the distances of each point to its assigned cluster
- Choose the solution with the smallest sum

✔ Returns exactly K clusters
🚫 Some clusters might be empty
🚫 Requires user to determine K
-> Elbow method:
- Run the algorithm with different values of K
- For each solution, sum the distances of each point to its assigned cluster
- Plot the resulting curve
![[Pasted image 20240521085546.png|400]]
✔ K-means clustering is unsupervised
✔ The Euclidian distance works well for data with homogeneous dimensions.

## Fisher Linear Discriminant Analysis
- Dimentionality reduction combined with clustering