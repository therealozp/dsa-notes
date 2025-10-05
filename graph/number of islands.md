#graph #bfs

use BFS (with a queue) to simplify things. whenever land is reached, greedily explore in all directions, sinking the island as you go so that you do not revisit it in the future.

```python
def numIslands(self, grid: List[List[str]]) -> int:
	m = len(grid)
	n = len(grid[0])
	
	islands = 0
	dirs = [(-1, 0), (1, 0), (0, -1), (0, 1)]
	
	for i in range(m):
		for j in range(n):
			if grid[i][j] == '1':
				islands += 1
				queue = deque([(i, j)])
				while queue:
					x, y = queue.popleft()
					if grid[x][y] != '1':
						continue
					grid[x][y] = 0
					for dx, dy in dirs:
						nx = x + dx
						ny = y + dy
						
						if nx >= m or nx < 0:
							continue
						
						if ny >= n or ny < 0:
							continue
						
						if grid[nx][ny] == 0:
							continue
						
						queue.append((nx, ny))
	return islands
```