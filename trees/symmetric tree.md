#dfs 
## recursive

```python
def isSymmetric(self, root: Optional[TreeNode]) -> bool:
	# recursively
	def sym(l, r):
		if not l and not r:
			return True
		if r and not l or l and not r:
			return False
		
		if l.val != r.val:
			return False
		
		return sym(l.right, r.left) and sym(l.left, r.right)
	
	return sym(root, root)
```

