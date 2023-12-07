#graph

The lowest common ancestor problem involves finding the nearest common ancestor of two nodes in a tree, keeping in mind that a node can be an ancestor of itself. Suppose that a `Node` datatype has the following implementation: 

```python
class Node: 
	def __init__(self, val: int, left: Node, right: Node): 
		self.val = val
		self.left = left
		self.right = right
```

and a tree is represented as these nodes chained together. Then, we can yield the lowest common ancestor by doing: 
1. initialize `path` arrays for the target variables
2. traverse the binary tree using depth-first search (DFS), appending the paths along the way
3. for each of the nodes yielded in the paths, we iterate over the two paths, and find where the paths begin to diverge. at that point, we understand that the **node before the divergence** is the ancestor.

```python
def dfsUtil(root, target, path):
	# print("current root: ", root.val)
	if not root:
		return None
		
	path.append(root.val)
	
	if root == target:
		return True
	if (root.left and dfs(root.left, target, path)) or (root.right and dfs(root.right, target, path)):
		# print('path found, exiting...')
		return True
	else:
		# root doesn't exist in subtree  
		# print('route does not exist. exiting...')    
		path.pop()
		return False
```

notes: 
- we have to handle the base case of the `dfsUtil`, meaning that the root does not exist. if the root is the target we are looking for, we return True right away. 

The condition set up for the loop is set up such that it will first: 
- explore the left branch of the node
- continue doing so, until one of its children **doesn't have a left branch**
- switch over to the right branch of that child
- continue searching
- and so on...
and as it goes, the `path` array will be filled out. if a path exists, it returns immediately, returning the path; if not, the path will be empty. 

The `main` function: 

```python
def lowestCommonAncestor(root: Node, p: Node, q: Node): 
	pathp = []
	pathq = []  

	# populate the path arrays
	dfs(root, p, pathp)
	dfs(root, q, pathq)
	
	# print(pathp)
	# print(pathq)

	i = 0
	# searching for the divergence
	while (i < len(pathp) and i < len(pathq)):
	  if pathp[i] != pathq[i]:
		break
	  i += 1

	return pathp[i-1]
```

