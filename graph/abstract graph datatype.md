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

## ADT
vertices and edges are positions that store elements: 
### accessor
- `e.endVertices()`: a list of the two endpoints of an edge `e`
- `e.opposite(v)`: the vertex opposite `v` on `e`
- `u.isAdjacentTo(v)`: returns true if and only if `u` and `v` are adjacent
- `*v`: returns a reference to an element associated with vertex `v`
- `*e`: returns a reference to an element associated with edge `e`
### updating
- `insertVertex(o)`: insert a vertex storing element `o`
- `insertEdge(v, w, o)`: inserts an edge `(v, w)` storing element `o`
- `eraseVertex(v)`: remove vertex `v` (and all its incident edges)
- `eraseEdge(e)`: remove edge `e`
### iterable collection methods
- `incidentEdges(v)`: returns a list of edges incident to `v`
- `adjacentVertices(v)`: lists all vertices that share an edge with vertex `v`
- `vertices()`: returns all vertices in the graph
- `edges()`: list of all edges in the graph

## representations
### adjacency list
- vertices are stored as **records/objects**
- every vertex stores a **list** of adjacent vertices
- allows the storage of additional data on vertices
- if edges are also objects, then can store additional data (such as edge **weight**)

![[Pasted image 20240414200826.png]]
### adjacency matrix
- 2D matrix
- rows represent source vertices
- columns represent destination vertices
- data on edges and vertices stored externally

```cpp
    0 1 2 3 4 5
0 - 0 0 1 3 0 0
1 - 0 0 0 5 0 0
2 - 1 0 0 2 1 4
3 - 3 5 2 0 7 0
4 - 0 0 1 7 0 2
5 - 0 0 4 0 2 0
```
### incidence matrix
- two-dimensional boolean matrix
- rows represent vertices, columns represent edges
- entries indicate whether vertex at a row is incident to an edge at a column

## time complexities

| operation            | adjacency list               | adjacency matrix                                                  | incidence matrix                                             |
| -------------------- | ---------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------ |
| memory               | $O(V+ E)$                    | $O(V^2)$                                                          | $O(V\times E)$                                               |
| adding vertex        | $O(1)$                       | $O(V^2)$                                                          | $O(V\times E)$                                               |
| adding edge          | $O(1)$                       | $O(1)$                                                            | $O(V\times E)$                                               |
| removing vertex      | $O(E)$                       | $O(V^2)$                                                          | $O(V\times E)$                                               |
| removing edge        | $O(V)$                       | $O(1)$                                                            | $O(V\times E)$                                               |
| check node adjacency | $O(E)$                       | $O(1)$                                                            | $O(V)$ - should be $O(E)$                                    |
| pros                 | constant time additions      | constant look up times, constant edge addition                    | ease of incidence calculations, interesting math qualities   |
| cons                 | slow removal for list search | slow to add/remove vertices, because matrix resize/copy is needed | slow to add/remove vertices/edges because matrix resize/copy |
## implementation
### edge list
the below implementation assumes that the collection of vertices and edges are realized with doubly-linked lists. uses $O(V + E)$ space.
**vertex object**: 
- element
- reference to position in a **vertex sequence**

**edge object**:
- element
- origin: vertex object
- destination: vertex object
- reference to position in **edge sequence**

| operation                                       | time   |
| ----------------------------------------------- | ------ |
| `vertices()`                                    | $O(V)$ |
| `edges()`                                       | $O(E)$ |
| `endpoints()`, `opposite()`                     | $O(1)$ |
| `incidentEdges()`, `isAdjacentTo()`             | $O(E)$ |
| `isIncidentOn()`                                | $O(1)$ |
| `insertVertex()`, `insertEdge()`, `eraseEdge()` | $O(1)$ |
| `eraseVertex()`                                 | $O(E)$ |
### adjacency list
relies on an **incidence sequence** for each **vertex**
- sequences of references to `edge` of incident edges
augmented edge objects
- references to associated positions in the **incidence sequence** of the end **vertex**

| operation                                       | time                        |
| ----------------------------------------------- | --------------------------- |
| `vertices()`                                    | $O(V)$                      |
| `edges()`                                       | $O(E)$                      |
| `endpoints()`, `opposite()`                     | $O(1)$                      |
| `v.incidentEdges()`                             | $O(\deg(v))$                |
| `v.isAdjacentTo(w)`                             | $O(\min(\deg(v), \deg(w)))$ |
| `isIncidentOn()`                                | $O(1)$                      |
| `insertVertex()`, `insertEdge()`, `eraseEdge()` | $O(1)$                      |
| `eraseVertex()`                                 | $O(\deg(v))$                |
## adjacency matrix
uses space $O(n\times n)$.
augmented **vertex objects**: 
- integer key (index) associated with the vertex
2D-array adjacency array
- reference to edge object for adjacent vertices
- null for non-adjacent vertices

| operation                          | time     |
| ---------------------------------- | -------- |
| `vertices()`                       | $O(V)$   |
| `edges()`                          | $O(V^2)$ |
| `endpoints()`, `opposite()`        | $O(1)$   |
| `isAdjacentTo()`, `isIncidentOn()` | $O(1)$   |
| `incidentEdges()`                  | $O(V)$   |
| `insertEdge()`, `eraseEdge()`      | $O(1)$   |
| `insertVertex()`, `eraseVertex()`  | $O(V^2)$ |
## subgraphs
a subgraph **S** of a graph **G** is a graph such that: 
- the vertices of **S** are a subset of the vertices of **G**.
- the edges of **S** are a subset of the edges of **G**.

a **spanning subgraph** of **G** is a subgraph that contains all the vertices of **G**.

a **tree** is an undirected graph **T** such that: 
- **T** is connected
- **T** has no cycles
this is similar to the tree family of binary trees, AVL trees; the only difference is that these are **rooted**.

a **forest** is an undirected graph without cycles, meaning that the connected components of a forest are trees.

## spanning tree
a spanning tree of a connected graph, is a spanning subgraph that is a tree. or, in other words, a spanning subgraph that is connected and has no cycles. these are **NOT** unique, unless the graph itself is a tree.

similarly, a spanning forest of a graph is a spanning subgraph that is a forest. 

these concepts are especially useful for designing communication networks.