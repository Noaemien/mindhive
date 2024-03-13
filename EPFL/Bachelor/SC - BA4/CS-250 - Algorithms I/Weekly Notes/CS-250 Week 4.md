---
Week: 4
Themes: 
aliases: 
Lecture1: true
Lecture2: false
Exercises: false
Quiz:
---

# Notes

## Lecture 1

### Midterm content
Everything up to and including binary search trees
No dynamic programming

### Last week recap

#### Heaps
A **Heap** is a binary tree where where the key of each parent is >= than key of children (Max Heap, also exists Min Heap)
![[Pasted image 20240312132500.png|300]]



### Stacks (LIFO)
Insert operation: Push(S, x)
Delete operation: Pop(S)

Can be used in code with {[()]} structures:
Example:
Push on opening bracket, pop on closing bracket to always know at which level we are in code.
![[Pasted image 20240312140131.png|500]]

### Queues (FIFO)
Insert operation: Push(S, x)
Delete operation: Pop(S)

### Linked List
Can be single linked or double linked (Each link points only to next or also points to prev)
![[Pasted image 20240312144844.png|500]]


List-search: Complexity O(n)
Insert (at head): Complexity O(1)



## Lecture 2

### Binary Search Trees
#### Key Property

If $y$ is in the left subtree of $x$ then $y$.key $<$ $x$.key
If $y$ is in the right subtree of $x$ then $y$.key $\ge$ $x$.key

#### Min and Max

Minimum is located in left most node
Maximum is located in right most node

#### Successor

Successor of node $x$ is the node $y$ such that $y$.key is the: "smallest key" > $x$.key

#### Printing Inorder
Print left subtree recursively
Print root
Print right subtree recursively

