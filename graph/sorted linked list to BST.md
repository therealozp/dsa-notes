#divide-and-conquer #tree #linked-lists 

```python
def sortedListToBST(self, head: Optional[ListNode]) -> Optional[TreeNode]:
	# divide and conquer + slow and fast
	def create_tree(head):
		if not head:
			return head

		if not head.next:
			return TreeNode(head.val)

		prev = None
		slow = head
		fast = head

		while fast and fast.next:
			prev = slow
			slow = slow.next
			fast = fast.next.next

		root = TreeNode(slow.val)
		prev.next = None

		left_subtr = create_tree(head)
		right_subtr = create_tree(slow.next)

		root.left = left_subtr
		root.right = right_subtr

		return root

	return create_tree(head)
```