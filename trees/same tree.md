#dfs #bfs #tree
two trees are considered the same if they share the same structure and all sub-nodes have the same values with the other tree's corresponding nodes.
## recursive

```python
def dfs(tree1, tree2):
	if not tree1 and not tree2:
		return True
	elif (not tree1 and tree2) or (tree1 and not tree2):
		return False

	if tree1.val != tree2.val:
		return False

	flag = True
	flag = flag and dfs(tree1.left, tree2.left)
	if not flag:
		return False
	flag = flag and dfs(tree1.right, tree2.right)
	return flag
	
def isSameTree(self, p, q):
	return dfs(p, q)
```

## iterative

```python
def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]:
	pp = deque([p])
	qq = deque([q])
	
	while pp or qq:
		np = pp.popleft()
		nq = qq.popleft()
		
		if (not np and nq) or (not nq and np):
			return False
		elif not np and not nq:
			continue
		
	   if np.val != nq.val:
			return False

		pp.append(np.left)
		qq.append(nq.left)

		pp.append(np.right)
		qq.append(nq.right)
	return True
```

