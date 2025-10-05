#divide-and-conquer #dfs

```python
def isValidBST(self, root: Optional[TreeNode]) -> bool:
	# cannot call level-by-level because 4 -> 2 -> 5 is wrong, but will evaluate to correct
	# check each individual subtree correctness -> go back to root?

	def validate(curr):
		# return subtree valid, max, min
		# what is the base case? -> when leaf node
		if not curr:
			return True, -float('inf'), float('inf')
		
		vl, mxl, mnl = validate(curr.left)
		if not vl or mxl >= curr.val:
			return False, float('inf'), -float('inf')
		
		vr, mxr, mnr = validate(curr.right)
		if not vr or mnr <= curr.val:
			return False, float('inf'), -float('inf')

		return True, max(curr.val, mxr, mxl), min(curr.val, mnr, mnl)

	res, _, _ = validate(root)
	return res
```

