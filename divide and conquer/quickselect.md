#divide-and-conquer #binary-search

this algorithm is normally used to solve **$k$-th smallest/largest** element problems, where instead of the normal $O(n\log n)$ runtime achieved with **heaps or sorting**:
- the average case runtime for quickselect is $O(n)$
- the worst-case runtime for quickselect is $O(n^2)$

functionally, quickselect is a combination of techniques found in binary search **and** quicksort, where:
- it is similar to quicksort in the way it uses partitioning to separate elements into different partitions of **larger or smaller** than the pivot
- and similar to binary search in the way it abandons either partition if the answer is not found.

```python
def findKthLargest(self, nums: List[int], k: int) -> int:
	index_to_find = len(nums) - k
	low = 0
	high = len(nums) - 1
	while low < high:
		pivot = nums[high]
		ptr = low
		for i in range(low, high + 1):
			if nums[i] < pivot:
				nums[ptr], nums[i] = nums[i], nums[ptr] # swap
				ptr += 1

		# swap last with insertion point
		nums[high], nums[ptr] = nums[ptr], nums[high]
		if index_to_find >= ptr:
			low = ptr + 1
		else:
			high = ptr - 1

	return nums[index_to_find]
```

