#linked-lists #heap
whenever we have a number of items that requires to be combined and sorted, or just continuously querying min/max items, using heaps are a good idea to consider. in this case, we want to merge k sorted linked list, with each node having the properties: 

```python
class ListNode: 
	def __init__(self, val=0, next=None): 
		self.val = val
		self.next = next
```

also whenever we use a heap to maintain a data structure, it is also good practice to **overload the < operator**. we can achieve this with `ListNode` by adding the following;

```python
	def __lt__(self, other): 
		return self.val < other.val
```

this generally helps the `heapq` run the `heapify()` and related functions. in this specific problem, we are trying to merge/sort the $k$ provided linked lists, so an approach using an array buffer can be done as follows: 

```python
def mergeKLists(lists: List[Optional[ListNode]]) -> Optional[ListNode]:
	end = []
	# push all node values onto the node by repeatedly iterating
	for head in lists:
		curr = head
		while curr:
		heappush(end, curr.val)
		curr = curr.next
	
	head = None
	curr = head
	prev = None
	# traverse and construct the linked list until the end array is empty
	while end:
		curr = ListNode(heappop(end))
		if prev:
			prev.next = curr
		else:
			head = curr
		prev = curr
	
	return head
```

because inserting and popping items off of heaps take $O(log(n))$ time, and there are $n$ nodes, we get the **time complexity** to be $O(nlog(n))$.

## further optimizations
### cleaner traversal
note that for the second part of reconstructing the linked list, another possible strategy to use instead of the **three pointer technique** is to use a **dummy node**: 

```python
dummy = ListNode()
curr = dummy

while end:
	curr.next = ListNode(heappop(end))
	curr = curr.next

return dummy.next
```

the point of this is that the dummy will act as the original trailing pointer, and from that point onward, `curr` will be it's own trailing pointer. `dummy.next` is basically pointing at the head of the linked list, so this works as well. 
### push only the head of $k$ lists, and process subsequent nodes later
with the overloading of the `__lt__` operator, instead of pushing the entire thing onto the buffer array, we only need to have 3 items (the heads) at one time. this can be achieved using: 

```python
def mergeKLists(self, lists):
        min_heap = []
        # Initialize the heap
        for l in lists:
            if l:  # Ensure the list is not empty
                heapq.heappush(min_heap, l)

        # Dummy head for the merged list
        dummy = ListNode(None)
        current = dummy

        # Merge the lists
        while min_heap:
            # Get the smallest node
            node = heapq.heappop(min_heap)

            # Add the smallest node to the merged list
            current.next = node
            current = current.next

            # If there's more in the list, add the next node to the heap
            if node.next:
                heapq.heappush(min_heap, node.next)

        return dummy.next
```

