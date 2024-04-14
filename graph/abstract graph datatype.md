a graph is a pair (V, E) where: 
- V is a set of nodes, called **vertices**
- **E** is a collection of **pairs of vertices**, called **edges**.
- both V and E are positions, and stores elements.

## edges
a directed edge is: 
- an **ordered** pair of vertices (u, v)
- first vertex u is the origin
- second vertex v is the destination
- e.g. a particular flight

an undirected edge is: 
- an **unordered pair** of vertices
- e.g. a flight route

## some terminology
- given an edge **a**, we say that u and v are endpoints of a. 
- given edge a, b, and c. we say that they are **incident** on v if they all share the same vertex v.
- given vertices u and v. we say they are **adjacent** if they are connected by an edge.
- given a node x, the **degree** of the node represent the number of edges incident at the node.
 - h and i are **parallel edges** when they both connect to the same endpoints.

- a **path** is a sequence of alternative vertices/edges, while a **simple path** is a path where all vertices/edges are distinct.
- a **cycle** is a **circular sequence** of vertices/edges, and the similar path logic goes for cycles.

## properties
in a **undirected graph** with $m$ edges and $n$ vertices:
1. total contribution of edges
- $\Sigma_{v}\text{deg}(v) = 2m$
- each edge is counted twice
2. a simple graph with $n$ vertices has $O(n^2)$ edges
- in an **undirected graph** with **no self loops** and **no parallel edges**: $m \leq \frac{n(n-1)}{2}$
- each vertex has degree of at most $n-1$

in a **directed** graph with $m$ edge and $n$ vertices:
1. total contribution of edges
- $\Sigma_{v}\text{indeg}(v) = \Sigma_{v}\text{outdeg}(v) = m$
- each edge contributes one **outdegree** to its origin and one **indegree** to its destination.

2. a simple graph has $O(n^{2})$ edges:
- in a directed graph with no self-loops and no parallel edges: $m \leq n(n-1)$
- each vertex has indegree and outdegree of at most $n-1$ each, and $\text{indeg}(v) + \text{outdeg}(v) \leq n-1$

