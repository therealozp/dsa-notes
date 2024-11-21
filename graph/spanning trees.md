a spanning tree of a connected graph, is a spanning subgraph that is a tree. or, in other words, a spanning subgraph that is connected and has no cycles. these are **NOT** unique, unless the graph itself is a tree.

similarly, a spanning forest of a graph is a spanning subgraph that is a forest. 

these concepts are especially useful for designing communication networks.

## minimum spanning tree (MST)
a spanning tree where sum of edge weights are as small as possible. or, in other words, it is a subset of edges that connects all possible vertices, without cycles, and minimum total edge weights.

### greedy approach

```
F = empty set
while (instance is not solved) {
	select an edge based on a locally optimal consideration
	if (adding edge to F is not acyclic) add;
	if (T = (V, F) is a spanning tree) {
		marked as solved
	}
}
```
### Prim's algorithm
the idea is to always greedily select the lowest-weighted edge connected to the current tree at every step. since the MST always include all vertices, the starting vertex actually does not matter - it could be anything.

- starts at one vertex
- adds an edge to the nearest, unconnected vertex
- repeat until a spanning tree is attained

pseudocode: 
```python
original vertices_set V
edges_set (F) = set()
vertices_set (Y) = set()

add arbitrary vertex to vertices_set
while Y != V:
	find the closest unvisited vertex
	if no cycle is formed:
		add selected vertex to Y
		add edge to F
```

actual code:
```python
def min_unselected(dist, selected):
	min_value = float('inf')
	min_index = 1
	for v in vertices:
		if visited[v] == False and dist[v] < min_key:
			min_value = dist[v]
			min_index = v
	return min_index

def prim(n: int, adj_matrix: int [][], edge_set: set):
	parent = [-1] * n
	dist = [float('inf')] * n
	visited = [False] * n
	
	mst_edges = []
	dist[0] = 0 # start from first vertex
	visited[0] = True
	
	# assuming that vertex 1 is already in the MST
	for i in range(n - 1): # executes n - 1 times
		# gets the closest unvisited vetex
		u = min_unselected(dist, selected)
		visited[u] = True
		
		# finds the closest edge that connects to the fetched vertex
		for v in vertices: 
			# if the edge doesn't exist, skip
			if not adj_matrix[u][v]:
				continue
			if visited[v] == False and adj_matrix[u][v] < dist[v]:
				dist[v] = adj_matrix[u][v]
				parent[v] = u
		
	for i in range(1, n):
		mst_edges.append(parent[i], i, adj_matrix[i][parent[i]])
```
this variant of Prim's algorithm yields a runtime of $O(n^2)$, where $n$ is the number of vertices

alternatively, another solution using priority queues:
```python
def tree(graph):
	# construct adjacency list
	mst_edges = []      
    pq = []
    visited = [False] * V
    res = 0
    heapq.heappush(pq, (0, 0, None)) # weight, vertex, parent
    while pq:
        wt, u, parent = heapq.heappop(pq)
        if visited[u]:
            continue  
        visited[u] = True  
        
	    if parent is not None:
		    mst_edges.append((parent, vertex, weight))
        for v, weight in adj[u]:
            if not visited[v]:
                heapq.heappush(pq, (weight, v, u))  
    return mst_edges
```
this method yields a runtime of $O(n+m\log n)$, where $m$ is the number of edges. this is due to the property of heaps, and we only check all of the edges + push/pop them off the heap. 

## Kruskal's algorithm
similar to Prim's in the sense that it greedily selects the lowest-weighted edge, Kruskal's process goes:
- sorts edge in non-decreasing order
- creates $n$ disjoint subset, 1 for every vertex
- greedily grabs the lowest-weighted edge
- if an edge connects 2 disjoint subsets, add edge to MST, and merge the 2 subsets.
- repeat

Kruskal's algorithm will most likely makes use of the disjoint set-union data structure, due to the merging of disjoint subsets.

```python
def kruskal(graph):
	sort original edge list in non-decreasing order 
	edges_set = set()
	k = 0
	while len(edges_set) < num_vertices - 1:
		k += 1
		e_ik = next best edge
		if edges_set + e_ik is acyclic: 
			edges_set.add(e_ik)
	return edges_set
```

another, more formal definition:
```python
def kruskal(n: int, m: int, edges):
	# n is number of vertices
	# m is number of edges
	edges.sort(lambda x: x.weight)
	F = empty_set
	initial(n) # initialize n disjoint subset
	while (len(F) < n - 1):
		e = edges.popleft()
		weight, u, v = e
		# p and q are the pointers for merge in the DSU
		p = find(u)
		q = find(v)
		if (p != q):
			merge(p, q)
			F.add(e)
	return F
```

time complexity analysis:
1. $O(m\log m)$ to sort edges
2. $O(n)$ to initialize different sets
3. while loop:
	- the `find` operations takes $O(\log m)$ time
	- worst case, every edge needs to be considered -> $O(m\log m)$ time complexity
4. to connect $n$ nodes requires at least $n - 1$ edges, so $O(m\log m) = \Theta(n\log n)$

the final TC for Kruskal's is $O(n + m\log m)$.

when it is fully connected, the number of edges $m$ is:
$$m=\frac{n(n-1)}{2}=\Theta(n^2)$$
so, the final time complexity is $\Theta(n^{2}\log n)$
## comparison
when the graph is very sparse e.g. $m$ is at the low end of edges limit, Kruskal's run in $O(n\log n)$ compared to Prim's $O(n^2)$

when the graph is dense, Kruskal's run in $O(n^{2}\log n)$ time, so it is slower than Prim's in this regard.

in conclusion, Kruskal when sparse, Prim when dense.