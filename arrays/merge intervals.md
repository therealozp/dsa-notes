#line-sweep #greedy
## traditional line-sweep algorithm

```python
def merge(self, intervals: List[List[int]]) -> List[List[int]]:
	# do a line sweep algorithm and 
	# check when it returns back to 0, that's the new endpoint
	curr_start = None
	curr_end = None

	event_counter = 0
	res = []
	hm = defaultdict(int)

	for s, e in intervals:
		hm[s] += 1
		hm[e] -= 1
	order = sorted(list(hm.keys()))
	for i in range(len(order)):
		time = order[i]
		if event_counter == 0:
			curr_start = time
		event_counter += hm[time]
		if event_counter == 0:
			curr_end = time
			res.append([curr_start, curr_end])
	return res
```
