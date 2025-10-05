## state space tree
should contain all possible tours that it can take. so, starting at a NULL node, we can choose our next node to be one of $n$ given nodes, and expand the SST based on the edges.

edges in the SST is limited by edges defined in original edge list.

## promising function
if a landmark is already visited, prune that option and not continue exploration.

### pseudocode

```python
# input: n number of nodes
# edge list E

def promising(node, path):
	if node in path:
		return False
	return True

def hamiltonian(node, path = []):
	if promising(node, path):
		if len(path) == n:
			print(path)
			return
		else:
			for each neighbor of node:
				path.append(neighbor)
				hamiltonian(neighbor, path)
				path.pop()		
```

## time complexity
we are looking for the upper bound. 
- at the first step, there is $n$ choices of nodes. 
- at the second step, since we visited one, there is $n-1$ choices.
- third, there is $n-2$
- and so on
so, the overall time complexity would be $O(n!)$, where $n$ is the number of nodes.

