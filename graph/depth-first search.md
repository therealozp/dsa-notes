## properties
DFS is a general technique that serves as a basis for graph traversals. a dfs of a graph **G**: 
- visits all the vertices and edges of G
- determines whether G is connected or not
- computes the connected components of G
- computes a [spanning forest](graph/abstract%20graph%20datatype#spanning%20tree) of G

can be extended to solve other graph problems: 
- check if path between two vertices exist
- if a graph is cyclic or acyclic

- `DFS(G, v)` will visit all vertices and edges in the connected component of `v`.
- the discovery edges labeled by the above function call will form a [spanning tree](graph/abstract%20graph%20datatype#spanning%20tree) of the connected component.

runtime: $O(V+E)$
## implementations
DFS will always use a stack to represent its traversal.
### recursive

```cpp
procedure DFS(G,v) {  
	visited.add(v) // where visited is a set 
	for (vertex u: G.adjacentVertices(v)) {  
		if (u not in visited) {  
			DFS(G, w)  
		}  
	}  
}
```
## iterative

```cpp
procedure DFS-iterative(G,v) {  
	stack S;  
	S.push(v)  
	while S is not empty {  
		w = S.pop()  
		if (w not in visited) {  
			visited.add(w);
			for (vertex x: G.adjacentVertices(w)) {  
				S.push(x)  
			}  
		}  
	}  
}
```

## time complexity
setting/getting a vertex/edge label takes $O(1)$ time, and the method `adjacentVertices()` is called once for each vertex.

runtime: DFS will run in $O(V+E)$ time, provided the graph is represented with an adjacency list.
- $\Sigma_{v}\deg(v)= 2m$
## applications
- finding connected islands (components)
- topological sorting (toposort)
- finding 2, 3-connected (edge or vertex) components
- finding bridges of a graph
- solving maze-problems
- maze generation using a randomized DFS
- biconnectivity in graph
- finding strongly-connected components
- plotting the Limit Set of a group
- planarity testing

