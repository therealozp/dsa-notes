#dp
## classical variant
### two-pointer-ish solution
```python
def maxProfit(self, prices: List[int]) -> int:
	n = len(prices)
	min_left = [prices[0]] * n
	max_right = [prices[-1]] * n

	for i in range(1, n):
		min_left[i] = min(min_left[i - 1], prices[i])

	for i in range(n - 2, -1, -1):
		max_right[i] = max(max_right[i + 1], prices[i])

	max_profit = 0
	for i in range(n):
		max_profit = max(max_profit, max_right[i] - min_left[i])
	return max_profit
```

### dynamic programming
```python
def maxProfit(self, prices: List[int]) -> int:
	n = len(prices)
	max_profit = 0
	min_so_far = prices[0]
	for i in range(n):
		max_profit = max(max_profit, prices[i] - min_so_far)
		min_so_far = min(min_so_far, prices[i])

	return max_profit
```

