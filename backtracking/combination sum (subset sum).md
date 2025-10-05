#backtracking 

```python
def combinationSum(self, candidates: List[int], target: int):
	candidates.sort()
	res = []
	def backtrack(curr_sum, start, path):
		if curr_sum == target:
			res.append(path.copy())
			return
		if curr_sum > target:
			return

		for i in range(start, len(candidates)):
			if curr_sum + candidates[i] > target:
				break

			path.append(candidates[i])
			backtrack(curr_sum + candidates[i], i, path)
			path.pop()

	backtrack(0, 0, [])
	return res
```
