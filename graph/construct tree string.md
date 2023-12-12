#graph 

Given a binary tree, how do we construct a string representation of the tree? This depends on the implementation, if the prompt asks to **ignore the null right node** or to make a **complete** representation. In any case, the algorithm provided is sufficient enough to traverse the tree using DFS and log out the elements accordingly. 

```python
def tree2str(self, root: Optional[TreeNode]) -> str:
	def dfsUtil(root, s):
		if not root:
			return

		s.append
		if root.left or (not root.left and root.right):
			s.append("(")
			dfsUtil(root.left, s)
			s.append(")")

		if root and root.right:
			s.append("(")
			dfsUtil(root.right, s)
			s.append(")")
	
	s = []
	dfsUtil(root, s)
	
	return "".join(s)
```

Approach: 
1. do a DFS of the entire tree
2. for the ignore null right case, check if whether there not a left child but there is a right child. the algorithm will DFS correctly anyways because there is a base case to handle the root argument being null
3. do the same for the right child. however, since the right child can be ignored (null), we do not have to check for it.

