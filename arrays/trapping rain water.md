#two-pointers

the sole, key insight to this problem is realizing that: the maximum amount of water that we can trap at a given index $i$, involves:
- finding the maximum wall height on the left
- finding maximum wall height on the right
- the height of the current wall

this is because, consider the following:

```txt
ooo
o
oo
```

given the index 1, we can visually see that 1 unit of water can be trapped. following the traditional two-pointer approach will not work here, as traditional 2p fails when there is a "runaway", aka a boundary where water is untrappable from that point.

```txt
ooooo <- "runaway" here
ooo
oo
ooo
oo
o
```
## $O(n)$ memory with array

```python
def trap(self, height: List[int]) -> int:
	n = len(height)
	max_left = [height[0]] * n
	max_right = [height[-1]] * n
	for i in range(1, n):
		max_left[i] = max(max_left[i - 1], height[i])
	for i in range(n - 2, -1, -1):
		max_right[i] = max(max_right[i + 1], height[i])
	total_water = 0

	# at each given index, the trappable water is given by:
	# min(maximum wall height on left, maximum wall height on right) - current wall height
	for i in range(n):
		total_water += max(0, min(max_left[i], max_right[i]) - height[i])

	return total_water
```

## $O(1)$ memory using two pointers
in this case, the two pointers are used as an absolutely auxiliary role to optimizing the above approach. the 2ps are used to keep track of the `max_left` and `max_right` variables exclusively, and the water is updated based on the current `l` and `r` pointers.

```python
def trap(self, height: List[int]) -> int:
	n = len(height)
	l = 0
	r = n - 1

	ml = height[l]
	mr = height[r]

	total_water = 0
	while l < r:
		if ml < mr:
			l += 1
			ml = max(ml, height[l])
			total_water += ml - height[l]
		else:
			r -= 1
			mr = max(mr, height[r])
			total_water += mr - height[r]
	return total_water
```

