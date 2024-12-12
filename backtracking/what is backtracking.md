an algorithm strategy based on graph traversal, somewhat related to greedy algorithms. the main idea involves: 
- considering all possible options, which define a decision tree
- traverse this **state graph** using DFS
- unlike greedy, this explores all options instead of only the best one.

because it considers all possible solutions, the decision tree is usually exponential. steps usually follow the format:
- form the state-space tree
- find the promising function
- perform dfs
- during dfs, evaluate if continuing to children leads to a promising output
	- if yes, keep visiting
	- if not, **prune the tree** (backtrack to parent)
- solution is found as we reach the leaves of the SST.
## state space graph
a directed graph which maps all decisions and intermediate steps for the problem. constructed by doing a **pre-order** traversal.
## promising function
the key to backtracking is depth-first search, while checking if the current set of input satisfies the problem requirements (called pruning). to this end, we say that a node is **non-promising** if visiting it leads to a dead-end.
## non-naive backtracking
because state space trees are often too big to do a naive traversal, there are a couple of pointers to keep in mind when considering a backtracking approach:
- don't construct the entire graph. instead, keep track of the location and the candidates.
- don't revisit the same state
- **prune the branch** on non-promising input
- order of exploration matters, and depends on the different types of problems. we always want to either:
	- explore the most promising candidates first and stop when an answer is reached
	- explore candidates that can eliminate the most possibilities
- heuristic backtracking, where we only explore the most likely candidates.

```python
def backtrack(Node n):
	if n is promising:
		if solution found at n:
			write solution
		else:
			for child_node in n.children:
				backtrack(child_node)				
```
