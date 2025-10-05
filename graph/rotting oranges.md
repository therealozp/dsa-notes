#bfs #graph 

```python
def orangesRotting(self, grid: List[List[int]]) -> int:
	minutes = 0
	m = len(grid)
	n = len(grid[0])
	dirs = [(0, -1), (1, 0), (0, 1), (-1, 0)]
	fresh = 0

	queue = deque()
	for i in range(m):
		for j in range(n):
			if grid[i][j] == 2:
				queue.append((i, j))
			elif grid[i][j] == 1:
				fresh += 1
	# if no fresh oranges remaining
	if fresh == 0:
		return 0

	while queue:
		new_queue = deque()
		if fresh == 0:
			break
		minutes += 1
		while queue:
			i, j = queue.popleft()
			for di, dj in dirs:
				ni, nj = i + di, j + dj
				# check out of bounds
				if ni < 0 or ni >= m or nj < 0 or nj >= n:
					continue

				# check if that square has a fresh orange. if yes, make it 2 and subtract.
				if grid[ni][nj] == 1:
					grid[ni][nj] = 2
					fresh -= 1
					new_queue.append((ni, nj))
		queue = new_queue

	if fresh != 0:
		return -1
	return minutes
```
