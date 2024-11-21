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

pseudocode: 
```python
edges_set (F) = set()
vertices_set (Y) = set()

add arbitrary vertex to vertices_set
while Y != V:
	for each edge:
		select a vertex in V - Y nearest to Y
	if no cycle is formed:
		add selected vertex to Y
		add edge to F
```

actual code:
```python
def prim(n: int, weight_matrix: int [][], edge_set: set):
	nearest = [0] * n
	dist = [0] * n

	# assuming that vertex 1 is already in the MST
	for i in range(2, n):
		nearest[i] = 1
		dist[i] = weight_matrix[1][i]

	while (n > 0):
		min_dist = float('inf')
		# finds the nearest vertex available
		for i in range(2, n):
			if  0 <= dist[i] < min_dist:
				min_dist = dist[i]
				nearest_vertex = i
		
		edge = edge connecting nearest_vertex and nearest[nearest_vertex]
		edge_set.add(edge)
		
		dist[i] = -1
		for i in range(2, n):
			if W[i][nearest_vertex] < dist[i]:
				dist[i] = W[i][nearest_vertex]
				nearest[i] = nearest_vertex
```
this variant of Prim's algorithm yields a runtime of $O(n^2)$, as there are nested loops of size $n$.

alternatively, another solution using priority queues:
```python
def tree(V, E, edges):
    adj = [[] for _ in range(V)]
    for i in range(E):
        u, v, wt = edges[i]
        adj[u].append((v, wt))
        adj[v].append((u, wt))
        
    pq = []
    visited = [False] * V
    res = 0
    heapq.heappush(pq, (0, 0))
    while pq:
        wt, u = heapq.heappop(pq)
        if visited[u]:
            continue  
        res += wt  
        visited[u] = True  
        for v, weight in adj[u]:
            if not visited[v]:
                heapq.heappush(pq, (weight, v))  
    return res  
```
this method yields a runtime of $O(E\log E)$, where $E$ is the number of edges. this is due to the property of heaps, and we only check all of the edges + push/pop them off the heap. 

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
5. when it is fully connected, the number of edges $m$ is:
$$m=\frac{n(n-1)}{2}=\Theta(n^2)$$
so, the final time complexity is $\Theta(n^{2}\log n)$
## comparison
when the graph is very sparse e.g. $m$ is at the low end of edges limit, Kruskal's run in $O(n\log n)$ compared to Prim's $O(n^2)$

when the graph is dense, Kruskal's run in $O(n^{2}\log n)$ time, so it is slower than Prim's in this regard.

in conclusion, Kruskal when sparse, Prim when dense.