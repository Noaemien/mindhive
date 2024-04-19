---
Week: 3
Themes:
  - Matrix multiplication
  - Heap
  - Heap Sort
  - Data Structures
aliases: 
Lecture1: true
Lecture2: true
Exercises: true
Quiz: true
---

# Notes

## Lecture 1

## Lecture 2

### Heaps

#### Definition
Heaps are binary tree-like data-structures
**Max-heap property:** Every child is smaller than its parent (also **min-heap property**)
**Height of node:** Longest path from node to a leaf
**Height of heap:** Height of the root

![[Pasted image 20240306115022.png|500]]
Can be represented as array:
![[Pasted image 20240312132719.png|300]]

#### Max Heapify
Merges two heaps + 1 element into one heap.
##### Algorithm
- Compare $A[i]$, $A[LEFT(i)]$ , $A[RIGHT(i)]$
- If necessary, swap $A[i]$ with largest of two children to preserve heap property
- Continue this process of comparing and swapping down the heap until subtree rooted at $i$ is max-heap





