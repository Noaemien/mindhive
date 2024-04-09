
# What is Dynamic Programming (DP)

## The main idea behind dynamic programming

Remember calculations that are already made in order to save enormous amounts of computation

## Key elements in designing a DP-algorithm

**Optimal substructure:**
> Show that a solution to a problem consists of *making a choice*, which leaves one or several subproblems to solve and the optimal solution solves the subproblems optimally


**Overlapping subproblems:**
> A naive recursive algorithm may revisit the same (sub)problem multiple times
> *Top-down with memoization:*
> 	Solve recursively but store each result in a table
> *Bottom-up:*
> 	Sort the subproblems and solve the smaller ones first; that way, when solving a subproblem, have already solved the smaller subproblems we need



# Rod Cutting

![[Pasted image 20240326131052.png]]
**Optimal substructure:**
To obtain an optimal solution, the remaining piece needs to be cut in an optimal way

If we let $r(n)$ be the optimal revenue from a rod of length n, we can express $r(n)$ recursively as follows:
$$r(n) = \begin{cases}
0 & \text{if } n = 0 \\ \\
\text{max}_{1\leq i\leq n} \{p_{i}+ r(n-i)\} & \text{otherwise if }\ge1
\end{cases}$$



**Pseudocode:**
![[Pasted image 20240326132447.png|500]]
r[i] is as defined above and s[i] is optimal solution for subproblem of size i


# Matrix Chain Multiplication

# Longest Common Sub-sequence (LCS)

## Definition
Given two sequences X and Y find a sub-sequence common two both whose length is the longest. 
> [!NOTE]
>*A sub-sequence doesn't have to be consecutive, but it has to be in order*

### Example

![[Pasted image 20240409135945.png|200]]
Here holly is the LCS

## DP Solution
If we start at the end of both sequences we can move left step by step. 
- If the letter in the same, put it in the sub-sequence. 
- If letter is not the same, optimal sub-sequence can be obtained by moving a step to the left in one of the words.
### Optimal substructure
![[Pasted image 20240409141954.png|500]]

### Recursive formula
$c[i,j] =$ length of LCS of $X_{i} \text{ and } Y_{i}$ 

$$c[i,j] = \begin{cases}
0 & \text{if } i = 0 \text{ or }j=0 \\
c[i-1,j-1] + 1 & \text{if }i,j>0 \text{ and }x_{i}=y_{j} \\
\text{max}(c[i-1, j], c[i,j-1]) & \text{if } i,j > 0 \text{ and } x_{i}\ne y_j
\end{cases}$$
### Recording optimal solution
Store optimal choices in an additional array $b[i,j]$ 
![[Pasted image 20240409144235.png|500]]

### Pseudocode

![[Pasted image 20240409142618.png|400]]


# Optimal binary search tree

## Definition
Given sequence K = ($k_{1}, k_{2}, \dots, k_{n}$) of n distinct keys, sorted ($k_{1}<k_{2}< \dots < k_{n}$)
We want to build a binary search tree from the keys.
For $k_{i}$ we have probability $p_{i}$ that search is for $k_i$.
Want BST with minimum expected search cost.
$$E[\text{Search cost in T}] = 1 + \sum^{n}_{i=1} \text{depth}_{T}(k_{i})p_{i}$$

Optimal BST might not have smallest height

## DP Solution
A BST can be build by first picking the root and then building optimal subtrees recursively.

### Recursive formula
$e[i,j] =$ expected search cost of optimal BST
![[Pasted image 20240409145858.png|500]]
Can be top down or bottom up with memoisation.

### Pseudocode
For bottom-up ($\Theta(n^{3})$ ):
![[Pasted image 20240409150039.png|500]]