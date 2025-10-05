
```python
def largestVariance(self, s: str) -> int:
	n = len(s)
	hm = Counter(s)
	max_var = 0

	for i in range(26):
		for j in range(26):
			c1 = chr(ord('a') + i)
			c2 = chr(ord('a') + j)

			curr_var = 0
			char_count = [0, 0]
			remaining_c2 = hm[c2]

			for char in s:
				if char == c1:
					char_count[0] += 1
				elif char == c2:
					remaining_c2 -= 1
					char_count[1] += 1
				if char_count[1] == 0:
					continue
				max_var = max(max_var, char_count[0] - char_count[1])
				if char_count[0] < char_count[1] and remaining_c2 > 0:
					char_count = [0, 0]

	return max_var
```