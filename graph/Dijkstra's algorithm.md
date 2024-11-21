#graph #bfs
let G be a **weighted** graph with the weight function $w: E \rightarrow R$, and
- V be the set of vertices
- E be the set of edges, where $E \leq V \times V$
- $w(u, v)$ be a **real-valued** weighted edge for between nodes $u, v$.

then, the distance $\sigma(u, v)$ from $u$ to $v$ is denoted such that:
- $\min(w(p): p \text{ from } u \text{ to } v)$ if a path exists
- $\infty$ otherwise.
or, in other words, $\sigma(u, v)$ is the **shortest path**.

Dijkstra's algorithm works ***only*** on graphs with **non-negative** edge weights, and is able to calculate the shortest path from a source vertex `s` from the graph to **all** other vertices.
## basis
Dijkstra's work based on 3 foundational lemmas: 
#### lemma 1: optimal substructure
any sub-path of a shortest path **is the shortest path**.
#### lemma 2: triangle inequality
for all vertices $u, v$ and $x$, $\sigma(u, v) \leq \sigma(u, x) + \sigma(x, v)$.
#### lemma 3: convergence
if $s\rightarrow u \rightarrow v$ is the shortest path in G, and if $d[u] = \sigma(s, u)$ at any time prior to relaxing the edges $(u, v)$, then $d[v] = \sigma(s, v)$ at all times afterwards.

to simplify, the lemma is stating that if the distance to $u$ is already minimized at any time before the edge $(u, v)$ is relaxed, then upon relaxation, that distance will become minimal as well.

## algorithm
- maintains an estimate $d[v]$ of length $\sigma(s, v)$ of the shortest path to each vertex $v \in G$.
- process all vertices, one by one, in some order. processing a vertex $u$ means finding a new path and updating $d[v]$ for all $v \in \text{adjacent}(u)$. this process is called **relaxation.**
- maintains a subset of vertices $S \subseteq V$, for which the true minimum distance is known. 
- the vertices are then selected one-by-one from $V -S$ (the vertices whose true minimum distance is **NOT** known) to be added into $S$. 
- the next-selected vertex is always a vertex $u$ in this subset where the estimated distance $d[u]$ is **minimum**.

```cpp
algorithm dijkstra() {
	for each v in all_vertices {
		d[v] = infinity;
		v.predecessor = NULL;
	}
	
	visited = [];
	queue = PriorityQueue(vertices);
	while (queue) {
		Vertex u = queue.extract_minimum();
		visited.add(u);
		for each vertex in u.adjacentVertices() {
			relax(u, v, w);
		}
	}
}

procedure relax(Vertex u, Vertex v, WeightFunction w) {
	if d[v] > d[u] + w(u, v) {
		d[v] = d[u] + w(u, v);
		v.predecessor = u;
		// modifying current distance of v
		// in the current queue
		decrease-key(v, d[v]);
	}
}
```

in legible python, dijkstra's would look something like:

```python
def dijkstra(source, target):
	dist = [float('inf')] * n
	visited = [False] * n
	
	pq = [(0, source)]
	while pq: # maximum will iterate E times
		weight, v = heappop(pq) # log(V) per insert/pop
		if v == target:
			return weight
		if visited[v]:
			continue
		visited[v] = True
		dist[v] = weight
		
		for neighbor, w in adj[node]: # maximum will iterate V times
			if dist[v] + w < dist[neighbor]:
				heappush(pq, (dist[v] + weight, neighbor)) # also log(V)
	
	return -1
```

note that in this implementation, the time complexity is $O((E +V)\log V)$ due to the repeated `heappush` and `heappop` calls. optimally, Dijkstra's would run in $O(V + E\log V)$.
## analysis
- initialization is executed $V$ times
- each vertex is processed **once**, so `extract_minimum()` is executed $V$ times in total
- the inner loop executed $E$ times, meaning `decrease_key()` is executed $V$ times.
$\rightarrow$ the runtime of Dijkstra's algorithm is $V\times T_{\text{extract\_{min}}}+ E\times T_{\text{decrease\_key}}$, which depends on the implementation of the priority queue.

| implementation | $T_{\text{extract\_min}}$ | $T_{\text{decrease\_key}}$ | total                   |
| -------------- | ------------------------- | -------------------------- | ----------------------- |
| array          | $O(V)$                    | $O(1)$                     | $O(V^2)$                |
| binary heap    | $O(\log V)$               | $O(\log V)$                | $O((V+E)\times\log(v))$ |
| Fibonacci heap | $O(\log V)$               | $O(1)$                     | $O(E\log V + V)$        |
## properties
1. the upper bound property: the invariant $d[v] \geq \sigma(s, v)$ will always hold for all $v$. 
2. when a vertex is added to $S$ (visited nodes), $d[u] = \sigma(s, u)$

