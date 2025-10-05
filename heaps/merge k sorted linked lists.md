#divide-and-conquer #linked-lists #heap 
## using heaps (cheating)
## using divide-and-conquer
an extremely similar approach to merge sort, but instead of sorting each individual elements, the `merge()` function takes care of re-organizing the two target linked lists such that they are sorted.

so, the key to this problem is realizing that you can split the `lists` into smaller sub-lists, and then continuously merging them into one another. then, you merge the merged lists, and so on, until only 1 list remains.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
	def merge(head1, head2):
		head = ListNode()
		ptr1 = head1
		ptr2 = head2
		curr = head

		while ptr1 and ptr2:
			if ptr1.val < ptr2.val:
				curr.next = ptr1
				ptr1 = ptr1.next
			else:
				curr.next = ptr2
				ptr2 = ptr2.next
			curr = curr.next
		
		while ptr1:
			curr.next = ptr1
			ptr1 = ptr1.next
			curr = curr.next
		
		while ptr2:
			curr.next = ptr2
			ptr2 = ptr2.next
			curr = curr.next
		
		return head.next

	def dnc(lists):
		if len(lists) == 0:
			return None
		
		if len(lists) == 1:
			return lists[0]
		
		n = len(lists)
		mid = n // 2
		left = dnc(lists[:mid])
		right = dnc(lists[mid:])
		
		final = merge(left, right)
		return final
		
	return dnc(lists)
```