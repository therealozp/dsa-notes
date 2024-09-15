a variant of Dijkstra's algorithm, to find min cost path from a ***single source***. works by **constantly relaxing every edge in the graph**. Bellman - Ford works for graphs with **negative distance** as well, and its solution is extremely elegant. 

however, it is not as efficient as Dijkstra's. in a graph with $\mid V\mid$ vertices and $\mid E \mid$ edges, the runtime complexity of BF is $O(VE)$.

BF works best with edge lists, which make the solution all the more elegant.

```python
# assuming that the edge list is [(source, destination, weight)]
# n is the number of nodes
dist = [float('inf') for _ in range(n)]
dist[source] = 0

for i in range(n - 1):
	for u, v, w in edges:
		if dist[u] > dist[v] + w:
			dist[u] = dist[v] + w

return dist[dest]
```

a proof of correctness can be found on Wikipedia, but relaxing all edges $n - 1$ times guarantees that the path will always be minimum distance.