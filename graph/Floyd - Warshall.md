a shortest-path algorithm for ***multiple sources*** (i.e. from every node to every other node). for the same problem of finding all the minimum distances: 
1. Dijkstra's is the fastest. $O((V+E)\log(V)\times V)$
2. Floyd - Warshall's is next. $O(V^3)$
3. Bellman - Ford is least efficient. $(V^4)$

the main logic for Floyd - Warshall's algorithm to work is that it simply iterates over all possible triplets of nodes, and compressing each distance every loop so that the minimum distance is ensured.

```python
# assuming that the edge list is [(source, destination, weight)]
# n is the number of nodes
dist = [[float('inf') for _ in range(n)] for _ in range(n)]

for i in range(n): 
	dist[i][i] = 0

for u, v, w in edges: 
	dist[u][v] = w
	dist[v][u] = w

# REMEMBER THE ORDER!
for k in range(n): 
	for i in range(n):
		for j in range(n): 
			if dist[i][j] > dist[i][k] + dist[k][j]:
				dist[i][j] = dist[i][k] + dist[k][j]
```
