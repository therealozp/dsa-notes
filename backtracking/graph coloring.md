finding all the different ways to color an undirected graph having $n$ vertices at most $m$ different colors, so that no two adjacent vertices have the same color.

a straightforward solution can be to try $m$ different colors for at each level, until each possible color combinations have been exhausted for vertex $v_{n}$ at level $n$.\
## backtrack
this problem lends itself well to backtracking, because we can determine the non-promising nodes to prune very early on. namely, our pruning conditions is that if a vertex **adjacent to it** is already colored with the same color, we can skip that color entirely.

problem statement:
determine all ways in which vertices in an undirected graph can be colored, using only $m$ colors, so that vertices are not the same color

inputs:
$n$: the number of vertices
$m$: number of colors
$W$: $n\times n$ adjacency matrix, indexed from $1$ to $n$, where `W[i][j]` is true if there is an edge connecting these 2 vertices.

output:
`color` array of size $n$, where `color[i]` is the color assigned to the i-th vertex.

```python
def promising(i):
	for each neighbor of node i:
		if color[i] == color[neighbor]:
			return False
	return True
```

```python
def graph_color(i):
	color = 0
	if promising(i):
		if i == n:
			print(color)
			return
		else:
			for each c of colors:
				color[i] = c
				graph_color(i + 1)
```

