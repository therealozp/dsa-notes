given two sorted arrays of length $m$ and $n$, merge them together into one sorted array.
## $O(n)$ space
very simple to implement, we consider each element at a local scope and march it forward to the end for optimal decision.

## $O(1)$ space
in this variant, we are explicitly given that one array is bigger than the other. this means that we can use this property to modify everything **in-place** instead of having to create a buffer.

one challenge with this is: how can we shift the elements around, if the empty space is provided at the end? intuitively, two solutions come to mind:
- we iterate as normal, comparing elements at indices one-by-one. if we encounter a bigger element, we place that element in $A[i + n]$ and iterate.
- we continuously swap the elements back and forth, until we reach the end.

both of the above solutions will not work. when we offset it to the end, we are not really checking any elements after either of the two finishes. for that reason, merging something like `[1, 2, 3, 5, 6, 0]` and `[4]` will give you something like `[1, 2, 3, 4, 6, 5]` as a result.

among these, another clever trick is to **iterate backwards**. this is because, as you move backward, the space is now at the "front" relative to you. so, the normal logic of $O(n)$ space apply.

```python
def merge(self, nums1, m, nums2, n)
	i = m - 1
	j = n - 1
	while i >= 0 and j >= 0:
		if nums1[i] >= nums2[j]:
			nums1[i + j + 1] = nums1[i]
			i -= 1
		else:
			nums1[i + j + 1] = nums2[j]
			j -= 1
	while j >= 0:
		nums1[i + j + 1] = nums2[j]
		j -= 1
```
