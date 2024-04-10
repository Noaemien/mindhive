# What is a graph ?
A graph $G = (V, E)$ consists of 
- a vertex set V
- an edge set E that contain (ordered pairs of vertices)
A graph can be un-directed, directed, vertex-weighted, edge-weighted, etc...

![[Pasted image 20240410111843.png|500]]

# Storing a graph
## Adjacency list
- An array where each index represents a vertex, where we store the pointer to the head of a list containing its neighbors
- Size $|V|$, one list per vertex
- Vertex u's list has all vertices v such that $(u,v) \in E$ 
- In pseudo-code it is noted *G.Adj*
	- For an un-directed graph, if $b$ is in the list of $a$ then $a$ is in the list of $b$ ![[Pasted image 20240410112355.png|500]]
	- For a directed graph ![[Pasted image 20240410112528.png|500]]
## Adjacency matrix
- $|V| \times |V|$ matrix $A = (a_{ij})$ where $$a_{ij} = \begin{cases}
1 & \text{if }(i,j) \in E \\
0 & \text{otherwise}
\end{cases}$$![[Pasted image 20240410113141.png|500]]

## Comparison of adjacency list and matrix
![[Pasted image 20240410113414.png|500]]

# Traversing and searching a graph
## Breadth-First Search
**Input:** Graph $G = (V,E)$ and source vertex $s \in V$ 
**Output:** Distance $v.d$ from $s$ to $v$ for all $v \in V$

**Idea:** Send wave out from s. Hit all vertices 1 edge from s. Then all v 2 edges from s...

**Pseudo-code:** 
![[Pasted image 20240410113939.png|200]]
**Runtime:** $O(V+E)$
- $O(V)$ because each vertex enqueued at most once
- $O(E)$ because every vertex dequeued at most once and we examine (u,v) only when u is dequeued. Therefore, every edge examined at most once if directed and at most twice if un-directed.

BFS may not reach all vertices.
We can save shortest path tree by keeping track of the edge that discovered the vertex.

## Depth-First Search
**Input:** Graph $G = (V,E)$ directed or un-directed
**Output:** 2 timestamps on each vertex: v.d = discovery time and v.f = finishing time

**Idea:** 
- Methodically explore every edge
- Start over from different vertices as necessary
- As soon as we discover a vertex explore from it,
	- Unlike BFS, which explores vertices that are close to a source first

**Pseudo-code:**
![[Pasted image 20240410114755.png|200]]
![[Pasted image 20240410114836.png|320]]
**Runtime:** $\Theta(V + E)$
- $\Theta(V)$ because each vertex is discovered once
- $\Theta(E)$ because each edge is examined once if directed graph and twice if un-directed graph
### Vertex colors
- White -> undiscovered
- Grey -> discovered, but not finished (not done exploring it)
- Black -> finished (explored everything reachable from it)
### Classification of edges
- **Tree edge:** In the depth-first forest, found by exploring $(u,v)$
- **Back edge:** $(u,v)$ where u is a descendant of v
- **Forward edge:** $(u,v)$ where v is a descendant of u, but not a tree edge
- **Cross edge:** any other edge

### Parenthesis theorem
For all u,v exactly one of the following holds
1. u.d < u.f < v.d < v.f (or inverse order) and neither u and v are a descendant of each other
2. u.d < v.d < v.f < u.f and v is a descendant of u 
3. v.d < u.d < u.f < v.f and u is a descendant of v 

### White-path theorem
Vertex v is a descendant of u if and only if at time u.d there is a path from u to v consisting of only white vertices (except for u which was just colored gray)


# Topological Sort

**Input:** A directed acyclic graph (DAG) $G = (V,E)$
**Output:**  a linear ordering of vertices such that if $(u,v) \in E$, then u appears somewhere before v

> [!NOTE] Directed acyclic graphs
> A directed graph if acyclic if and only if a DFS of G yields no back edges

## Algorithm for topological sort
1. Call DFS(G) to compute finishing times v.f for all $v \in G.V$ 
2. Output vertices in order of decreasing finishing times
Example: 
![[Pasted image 20240410121252.png|500]]
No (real) need to sort as we can just output vertices as they finish and take reverse of list. Or put them in front of linked list when they finish, when done the list is in topologically sorted order.
Time $\Theta(V + E)$ (Like DFS)