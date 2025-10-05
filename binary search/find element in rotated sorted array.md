#binary-search
## invariant
in the array, because of its already-sorted nature, we can divide it into two halves: 
- one where it is in perfect sorted order
- the other contains the rotation point

doing this with a left and a right pointer, each two subarrays created any binary-search combination of the two will contain these two halves.
## solution
so, we can make use of it and contain our search range. so in this case:
- pick the middle element and check if it is our target.
- then, we try to find which half is "sorted"
	- if the sorted half is the same as the direction (i.e. left half sorted and target is smaller than the midpoint), limit it to the sorted half
	- else, find the target in the unsorted half

binary search only works if the range is sorted, because we can use range comparisons (`target < nums[mid]`) to safely eliminate half the search space.  

1. Identify which half is **sorted**.
2. Check if the `target` lies inside that sorted half’s range.
    - **If yes** → search that half (binary search logic works here).
    - **If no** → discard that half, search the other half (which will contain the rotation point but is still searchable on the next iteration).

```python
def search(self, nums, target):
	"""
	:type nums: List[int]
	:type target: int
	:rtype: int
	"""

	left = 0
	right = len(nums) - 1

	while left <= right:
		mid = (left + right) // 2
		if nums[mid] == target:
			return mid

		if nums[mid] >= nums[left]:
			if nums[left] <= target and target < nums[mid]:
				right = mid - 1
			else:
				left = mid + 1

		else:
			if nums[mid] < target and target <= nums[right]:
				left = mid + 1
			else:
				right = mid - 1

	return -1
```