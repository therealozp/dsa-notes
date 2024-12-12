## problem 1
a.
for each number in the denominations, the number of coins required to make a value of $n$ is dependent on the values before, all the way from $n - d_{1}$ to $n - d_{m}$. this is where the overlapping subproblem comes in, for example, to make coins for 5, 6, and 9 for the denominations 1, 2, and 4 respectively, they all have an overlapping subproblem of 4.

now, the key to the problem is realizing that, because each value of the denomination will only take 1 coin to complete, we are guaranteed to reach the optimal solution if we consider all of the values of `current_value - denomination`, as anything with that will just be equal to the amount of coins required to make that smaller value, +1 for the coin needed to reach it.

so, the recurrence relation of the problem becomes `dp[value] = min(dp[all of number of value - denomination]) + 1`

b.

| value     | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| min coins | 0   | 1   | 1   | 2   | 2   | 1   | 2   | 2   | 3   |
| 9         | 10  |     |     |     |     |     |     |     |     |
| 3         | 2   |     |     |     |     |     |     |     |     |
to explain, it is trivial that you would need 0 coins to make 0.
for 1 coins:
- using denomination 1: 0 needs 0 coins, so 1 now needs minimum 1 coins.
minimum coins needed for 1 is 1.

for 2 coins:
- using denomination 1: 1 needs 1 coins, so 2 now needs minimum 2 coins.
- using denomination 2: 0 needs 0 coins, so 2 now needs minimum 1 coins.
minimum coins needed for 2 is 1.

for 3 coins:
- using denomination 1: 2 needs 1 coins, so 3 now needs minimum 2 coins.
- using denomination 2: 1 needs 1 coins, so 3 now needs minimum 2 coins.
minimum coins needed for 3 is 2.

for 4 coins:
- using denomination 1: 3 needs 2 coins, so 4 now needs minimum 3 coins.
- using denomination 2: 2 needs 1 coins, so 4 now needs minimum 2 coins.
minimum coins needed for 4 is 2.

for 5 coins:
- using denomination 1: 4 needs 2 coins, so 5 now needs minimum 3 coins.
- using denomination 2: 3 needs 2 coins, so 5 now needs minimum 3 coins.
- using denomination 5: 0 needs 0 coins, so 5 now needs minimum 1 coins.
minimum coins needed for 5 is 1.

for 6 coins:
- using denomination 1: 5 needs 1 coins, so 6 now needs minimum 2 coins.
- using denomination 2: 4 needs 2 coins, so 6 now needs minimum 3 coins.
- using denomination 5: 1 needs 1 coins, so 6 now needs minimum 2 coins.
minimum coins needed for 6 is 2.

for 7 coins:
- using denomination 1: 6 needs 2 coins, so 7 now needs minimum 3 coins.
- using denomination 2: 5 needs 1 coins, so 7 now needs minimum 2 coins.
- using denomination 5: 2 needs 1 coins, so 7 now needs minimum 2 coins.
minimum coins needed for 7 is 2.

for 8 coins:
- using denomination 1: 7 needs 2 coins, so 8 now needs minimum 3 coins.
- using denomination 2: 6 needs 2 coins, so 8 now needs minimum 3 coins.
- using denomination 5: 3 needs 2 coins, so 8 now needs minimum 3 coins.
minimum coins needed for 8 is 3.

for 9 coins:
- using denomination 1: 8 needs 3 coins, so 9 now needs minimum 4 coins.
- using denomination 2: 7 needs 2 coins, so 9 now needs minimum 3 coins.
- using denomination 5: 4 needs 2 coins, so 9 now needs minimum 3 coins.
minimum coins needed for 9 is 3.

for 10 coins:
- using denomination 1: 9 needs 3 coins, so 10 now needs minimum 4 coins.
- using denomination 2: 8 needs 3 coins, so 10 now needs minimum 4 coins.
- using denomination 5: 5 needs 1 coins, so 10 now needs minimum 2 coins.
minimum coins needed for 10 is 2.

so, the final minimum number of coins needed to make 10 is 2

c. 
pseudocode:
```python
def min_coins(denom, n):
	dp = [-1] * (n + 1)
	dp[0] = 0
	
	for i in range(1, n + 1):
		min_coins = float('inf')
		for d in denom:
			if i - d >= 0:
				min_coins = min(min_coins, dp[i - d] + 1)
		dp[i] = min_coins
	return dp[n]
```

d.
**dynamic programming:** 
the time complexity of this approach is $O(\text{value}\times k)$, where $k$ is the number of denominations given. this is because the dynamic programming solution contains one nested loop inside of another, resulting in the discussed time complexity.

as for the space complexity, this would be $O(\text{value})$, as it uses a 1-dimensional array to keep track of the number of coins needed to make $i$.

**brute-force**:
the recursive solution involves trying every denomination and solving for the $\text{value} - \text{denomination}$ for each subproblem. that means, for every subproblem, the algorithm would have to make $k$ decisions at every single step, to either include the coin at $i$ or not. this means that the worst-case time complexity would be $O(k^\text{value})$, which is exponential.

however, the space complexity is not as bad. because the algorithm tries solving with $\text{value} - \text{denomination}$ at every step of the problem and stops when that difference goes below 0. that means the recursive stack will always be at worst $O(n)$, when it repeatedly subtracts the smallest possible denomination.
## problem 2
a. 
the entire key with this problem is that, at every given step, we are given 2 choices of how to proceed. we can either: 
- skip (not take) the item, lose out on all the benefit, but conserve some amount of weight
- take the item, and bear the weight.

that means, for each step of the problem, there are 2 clear choices we can make. so, the recurrence relation becomes
```python
dp(i, remaining_weight) = max(
		dp(i + 1, remaining_weight),
		benefit[i] + dp(i + 1, remaining_weight - weight[i]
		)
```

while we can write a top-down memoization algorithm, it would take up a lot more space in recursion stacks compared to the bottom-up iterative version. what this means for us is that we have to keep track at the maximum possible value we can get for any given item, at any given weight from $0\to \text{maximum weight}$.
b. 

```  python
all possible valid solutions: (total value, total weight, items): 
(0, 0, []) 
(6, 2, [0]) 
(10, 3, [1]) 
(16, 5, [0, 1]) 
(12, 4, [2]) 
(18, 6, [0, 2]) 
(22, 7, [1, 2]) 
(28, 9, [0, 1, 2]) 
(18, 5, [3]) 
(24, 7, [0, 3]) 
(28, 8, [1, 3]) 
(34, 10, [0, 1, 3]) 
(30, 9, [2, 3])
```

the algorithm correct outputs the maximum value: 34, total weight 10, and includes item 0, 1, and 3.

c.
pseudocode
```python
def bottom_up_knapsack(benefit, weight, capacity):
	dp = matrix of size(n * capacity + 1)
	for i in range(n):
		for cw in range(capacity + 1):
			if weight[i] > cw:
				# if it is over capacity, skip the item
				# unless it is the first item, in which case default to 0
				dp[i][cw] = 0
				if i > 0:
					dp[i][cw] = dp[i - 1][cw]
			else:
				skip = dp[i - 1][w]
				take = benefit[i] + dp[i - 1][cw - weight[i]]
				dp[i][cw] = max(skip, take)
	return dp[-1][-1]
```

d.
**dynamic programming**
this problem is solved bottom-up. the solution involves:
- constructing a matrix `dp` where `dp[i][cw]` represents the maximum benefit achievable using the first `i` items and a capacity of `cw` (current weight).

The algorithm iteratively fills the matrix as follows:

1. if the weight of current item exceeds current capacity, so the default choice is to skip the item (e.g. `dp[i-1][cw]`)
2. else the algorithm considers two options: **skip the item** or **include the item**. The value for `dp[i][cw]` is the maximum of these two choices:
    - skip: Use the value from `dp[i-1][cw]`.
    - take: Add the benefit of the current item to the value of the remaining capacity, `dp[i-1][cw - weight[i]]`.
3. solution is stored in `dp[n][cap]`, which represents the maximum benefit achievable with all $n$ items and the given capacity.

so, the time complexity and space complexity of this approach is $O(n \times \text{capacity})$, as it contains two nested loops, and involves initializing a 2D matrix to store results.

**brute force**
the brute-force approach involves exploring all possible subsets of the nnn items to determine the maximum benefit that fits within the capacity. this is achieved through recursion by either skipping or taking each item in the solution.

with the recurrence relation defined, it is clear that for each of the recursive steps, there are 2 choices we can make: either take, or skip. for $n$ items, the time complexity of the brute force solution becomes $O(2^n)$.

similar to problem 1, the recursive tree will only drill down until all of the items are exhausted, which leads to a space complexity of $O(n)$.

## problem 3, subproblem 1
a. 
the original graph:
![[Pasted image 20241121005919.png]]

b.
### prim's algorithm
using Prim's algorithm, the process of obtaining the MST is as follows: 
![[Pasted image 20241121010024.png]]
![[Pasted image 20241121010044.png]]
![[Pasted image 20241121010101.png]]
![[Pasted image 20241121010121.png]]
![[Pasted image 20241121010142.png]]
![[Pasted image 20241121010155.png]]
![[Pasted image 20241121010216.png]]
so, the final total weight of running Prim's algorithm is 12.

### Kruskal's algorithm
using Kruskal's algorithm to merge $n$ disjoint sets, the process looks as follows (each color indicates 1 disjoint set): 
![[Pasted image 20241121010435.png]]
![[Pasted image 20241121010500.png]]
![[Pasted image 20241121010524.png]]
![[Pasted image 20241121010548.png]]
![[Pasted image 20241121010618.png]]
![[Pasted image 20241121010646.png]]
![[Pasted image 20241121010740.png]]
![[Pasted image 20241121010804.png]]

c.
because Prim's algorithm involves checking each edge stemming from each vertex, it strictly loops over all different vertices in search of the closest unvisited node. the adjacency matrix would allow Prim's algorithm to make constant-time edge lookups, which is the key to remaining the $O(n^2)$ time complexity.

even better, considering that the network is a **dense** graph, where the number of edges scale quadratically with the number of vertices, the time complexity that Kruskal's experience scales from $O(E\log E)$ to $O(V^{2}\log V)$. in the end, Prim's is just much more optimized for the task at hand.

## problem 3, subproblem 2
a. 

distance table:

| iteration | A   | B   | C   | D   | E   | F   |     |     |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1         | 0   | inf | inf | inf | inf | inf |     |     |
| 2         | 0   | 4   | 2   | inf | inf | 6   |     |     |
| 3         | 0   | 3   | 2   | 10  | 12  | 5   |     |     |
| 4         | 0   | 3   | 2   | 8   | 10  | 5   |     |     |
| 5         | 0   | 3   | 2   | 8   | 10  | 5   |     |     |
| 6         | 0   | 3   | 2   | 8   | 9   | 5   |     |     |
| 7         | 0   | 3   | 2   | 8   | 9   | 5   |     |     |
| 8         | 0   | 3   | 2   | 8   | 9   | 5   |     |     |
| 9         | 0   | 3   | 2   | 8   | 9   | 5   |     |     |
| 10        | 0   | 3   | 2   | 8   | 9   | 5   |     |     |
| 11        | 0   | 3   | 2   | 8   | 9   | 5   |     |     |
each step priority queue and visited set:

```python
priority queue: [(0, 'A')]
set() 

priority queue: [(2, 'C'), (4, 'B'), (6, 'F')] 
{0} 

priority queue: 
[(3, 'B'), (6, 'F'), (4, 'B'), (10, 'D'), (12, 'E'), (5, 'F')] 
{0, 2} 

priority queue: [(4, 'B'), (6, 'F'), (5, 'F'), (10, 'D'), (12, 'E'), (8, 'D'), (10, 'E')] 
{0, 1, 2} 

priority queue: [(5, 'F'), (6, 'F'), (8, 'D'), (10, 'D'), (12, 'E'), (10, 'E')] 
{0, 1, 2} 

priority queue: [(6, 'F'), (10, 'D'), (8, 'D'), (10, 'E'), (12, 'E'), (9, 'E')]
{0, 1, 2, 5} 

priority queue: [(8, 'D'), (10, 'D'), (9, 'E'), (10, 'E'), (12, 'E')] 
{0, 1, 2, 5} 

priority queue: [(9, 'E'), (10, 'D'), (12, 'E'), (10, 'E')] 
{0, 1, 2, 3, 5} 

priority queue: [(10, 'D'), (10, 'E'), (12, 'E')] 
{0, 1, 2, 3, 4, 5} 

priority queue: [(10, 'E'), (12, 'E')] 
{0, 1, 2, 3, 4, 5} 

priority queue: [(12, 'E')] 
{0, 1, 2, 3, 4, 5}
```

the respective shortest paths:

```python
To A: A
To B: A -> C -> B
To C: A -> C
To D: A -> C -> B -> D
To E: A -> C -> F -> E
To F: A -> C -> F
```

b.
Prim's pseudocode:

```python
def prims(graph):
	# graph is an adjacency matrix
	mst_edges = []
	visited = [False] * n
	parent = [None] * n
	dist = [float('inf')] * n

	visited[0] = True
	dist[0] = 0
	
	for i in range(n - 1):
		u = the closest unvisited node
		visited[u] = True
		
		for v in range(n):
			if edge(u, v) does not exist: 
				continue
				
			if not visited[v] and graph[u][v] < dist[v]:
				# only worry about the value of the edge
				# so the dist[v] will continously be updated
				# with the lowest found edge weight connected to v
				dist[v] = graph[u][v]
				parent[v] = u
	
	for i in range(len(parent)):
		if parent[i] is not None:
			mst_edges.append(parent, i, weight)
```

Dijkstra's pseudocode:

```python
def dijkstra(graph, src):
    dist = [float('inf')] * num_vertices
    visited = set()
    
    min_heap = [(0, src)]
    distance of source = 0
	    
    while pq:
        weight, node = min_heap.pop()
        if node in visited:
            continue
        visited.add(node)
        dist[node] = weight
        
        for neighbor, w in graph[node]:
            if w + weight < dist[neighbor]:
                dist[neighbor] = w + weight
                min_heap.push((new_weight, neighbor))

    return dist
```

Prim's and Dijkstra's are both greedy algorithms used to find the shortest paths or minimum spanning trees, but they have different goals and computational characteristics.

- **Prim's algorithm**: finds the MST. it does not require a source node and works by incrementally adding the closest edge to the already connected part of the tree.
- **Dijkstra's algorithm**: finds the shortest path from a source node to all other nodes in a graph. it calculates the shortest path from the source to each vertex individually.

**complexity**
- **Prim's algorithm**:
    - time complexity is $O(n^2)$ with an adjacency matrix and $O(n + mlog⁡n)$ with a priority queue and adjacency list.
    - it is more efficient for dense graphs when implemented with an adjacency matrix.
- **Dijkstra's algorithm**:
    - time complexity is $O(n^2)$ with an adjacency matrix and $O((n+m)log⁡n)$ with a priority queue and adjacency list.
    - it is more efficient for sparse graphs when implemented with a priority queue and adjacency list.

c.
for Dijkstra's, the output is a cumulative distance from one node to the next, all the while recording the respective paths the source. this could be used for pathfinding purposes such as navigation, finding the most optimal path to take, or load balancing where the source node's position is known.

for Prim's, the output is simply a collection of edges that comprises the minimum spanning tree. since this is strictly the minimum edges, this could be used for cost optimization when installing new infrastructure (such as airports or railways), or making better chip designs where the paths from CPU to other components are optimized.