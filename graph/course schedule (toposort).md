#topological-sort

```python
def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
	graph = defaultdict(list)
	indegree = defaultdict(int)

	for course in range(numCourses):
		graph[course] = []
		indegree[course] = 0

	for course, prereq in prerequisites:
		graph[prereq].append(course)
		indegree[course] += 1

	queue = deque()
	# 0 indegree

	for node in indegree:
		if indegree[node] == 0:
			queue.append(node)

	# print(graph, queue)

	visited = set()

	while queue:
		node = queue.popleft()

		if node in visited:
			continue

		visited.add(node)

		while graph[node]:
			nb = graph[node].pop()
			indegree[nb] -= 1

			if indegree[nb] == 0:
				queue.append(nb)

	if len(visited) == numCourses:
		return True
	return False
```