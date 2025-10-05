#binary-search
## The problem

We’re given a **rotated, strictly ascending** array, e.g.:

```
[4, 5, 6, 7, 0, 1, 2, 3]
```

It’s really just a sorted array that’s been “cut” at some pivot and wrapped around.

Our goal: **find the index of the maximum element** (the “peak”).

## Observations

### Two sorted blocks

Any rotated sorted array (with no duplicates) can be split into two contiguous **blocks**:

- **Left block**: Starts at index 0, ends at the peak.  
    Every element in this block is **≥ nums[0]**.
    
- **Right block**: Starts at the pivot (minimum element), ends at the end of the array.  
    Every element in this block is **< nums[0]**.
    

Example with `nums[0] = 4`:

```
[4, 5, 6, 7]   [0, 1, 2, 3]
 ^ left block    ^ right block
```
## Key insight
The peak is **the last element in the left block**.

If we can decide whether a given `mid` index is in the left block or right block, we can narrow down where the peak is.
## Deciding the block

Compare `nums[mid]` to `nums[0]`:
- If `nums[mid] >= nums[0]` → `mid` is in the **left block**.
- If `nums[mid] < nums[0]` → `mid` is in the **right block**.

This works because:
- Left block values are all ≥ the first element.
- Right block values are all strictly less than the first element.

## Binary search procedure
We maintain two pointers:

```
left = 0
right = n - 1
```

We repeatedly:
1. Pick a **right-biased** midpoint:

```
mid = (left + right + 1) // 2
```

Right-biased means that when `left` and `right` are neighbors, `mid` will point to `right`. This prevents infinite loops.

2. Check which block `mid` is in:

- **Left block (`nums[mid] >= nums[0]`)**:  
	The peak is **at or after** `mid` → move `left = mid`.

- **Right block (`nums[mid] < nums[0]`)**:  
	The peak is **before** `mid` → move `right = mid - 1`.

3.  Repeat until `left == right`.

At the end, `left` (or `right`) is the index of the peak.

## Example walkthrough

Array:

```
nums = [4, 5, 6, 7, 0, 1, 2, 3]
```

`nums[0] = 4`

|left|right|mid|nums[mid]|Block?|Action|
|---|---|---|---|---|---|
|0|7|4|0|Right|r = 3|
|0|3|2|6|Left|l = 2|
|2|3|3|7|Left|l = 3|

End: `left = 3`, `nums[3] = 7` → peak found.

## Unrotated case

Example:

```
[0, 1, 2, 3, 4, 5, 6, 7]
```

Here, the **right block is empty**. Every `nums[mid] >= nums[0]`, so we always move `left` forward until it reaches `n - 1`. That’s the peak in an unrotated array.

## Pseudocode

```python
def find_peak(nums):
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right + 1) // 2  # right-biased
        if nums[mid] >= nums[0]:
            left = mid       # stay in left block
        else:
            right = mid - 1  # move to left block
    return left  # index of max element
```

## Visualization

Think of the array as two colored blocks:

```
Left block (green): ≥ nums[0]        Right block (red): < nums[0]
[ 4, 5, 6, 7 ]                       [ 0, 1, 2, 3 ]
```

Binary search **shrinks the range** to end exactly at the last green element.
