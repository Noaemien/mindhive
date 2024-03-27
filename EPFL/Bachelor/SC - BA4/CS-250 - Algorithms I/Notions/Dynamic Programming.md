
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

