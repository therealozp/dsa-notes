## problem 1
a. 
the problem asks us to compute the maximum number of people we can save, given the weight of the medicine and the number of people that can be saved with that vaccine.

for this problem, I opted with the **greedy** approach and greedily select the product with the highest **people saved-by-weight** ratio, as it allows me to choose ones that can save the most number of people w[]()hile not going over the weight limit. if the algorithm ever encounters a vaccine where taking the entire stock will overload the weight, i can take a smaller fraction of it.

b.
Python greedy implementation:
```python
# assuming vaccine is stored in a tuple (vaccine_name, weight, people_saved)
def solve(vaccines, weight_constraint=100):
	# first, sort the vaccines in non-increasing order based on the ratio
	# using the anonymous function for succintness
	vaccines.sort(key=lambda x: x[2] / x[1], reverse=True)
	result_set = []
	weight = 0
	people_saved = 0
	for name, w, p in vaccines:
		# we try to take everything, if it exceeds, then we take fraction
		if weight + w > weight_constraint:
			remaining_weight = weight_constraint - weight
			# we take the ratio of the remaining_weight versus remaining product
			# and multiply it by the original number
			savable = remaining_weight / w * p 
			weight += remaining_weight
			
			result_set.append((name, remaining_weight))
			people_saved += savable
			break
			
		weight += w
		people_saved += p
		result_set.append((name, w))

	return (result_set, weight, people_saved)
```

output:

```plaintext
Order: [('Smallpox', 10), ('Measles', 20), ('Yellow Fever', 50), ('Chickenpox', 20)]
Weight used: 100
People saved: 160000.0
```

c.
**Greedy**:
- The input sorting step by ratio costs $O(n\log n)$. The `lambda` function that calculates the ratio is ran in $O(1)$ time. 
- The `for` loop executes n times and the inner `if` statement is executed every time the loop is run, leading to an $O(n)$ loop.

Therefore, the total time complexity of the solution is $O(n\log n)$.

**Brute-force**:
- Since doing something like this would require all possible combinations of the vaccines array, we need to generate $2^n$ combinations and check each of them individually. So, the time complexity is $O(2^n)$.
- If there is ever a weight constraint violation, we need to go over the excluded subset of items and add in the fractional values to find the maximum. In an event where all the `weights` and `people_saved` are identical, we will have to check the entire array.

Therefore, the brute-force time-complexity is $\Omega(2^n)$. 

d. 
If fractions of vaccines are not allowed, the described approach **WOULD NOT** work. This is because, for some cases, taking the entire stock of the higher people-saved-by-weight ratio might leave us with some extra capacity, and we would be better off taking another stock with higher amount of total people saved.

consider this case, where capacity is 10 lbs, and with vaccines described as follows:

| vaccine | weight | people_saved |     |
| ------- | ------ | ------------ | --- |
| A       | 5      | 10000        |     |
| B       | 10     | 15000        |     |
in this case, while A has the higher people-saved-per-lbs ratio, taking all 10 pounds of B would ultimately save more people. hence, the greedy approach will not work.
## problem 2

a.
problem requirements: given 3 arrays of processing times, priority weights, and the penalty rate of each CPU task, we need to find a way to schedule these tasks such that the cost is minimized. the formula for the cost of each task is calculated by the following:
$$\sum{(\text{waiting time} + \text{processing time}) \times \text{priority weight} \times \text{penalty weight}}$$
**analysis:** 
since our goal is to minimize the total cost, and the fact that we are only allowed one task to be running at any given time, it would make sense to **sort the input based on the total weight**, which is the product of the last two terms listed above. by the same token, for tasks of the same cost, we should **run the one that takes less time first**.

each ms of waiting time will cost each task $t$: $\text{priority weight}_{t} \times \text{penalty weight}_{t}$. that means, we can minimize cost by running the most expensive functions first, and leave the cheaper ones to run later as the accumulated cost will be less compared to the any other order.

one more note is that we should also **include the processing time** into the greedy heuristic. consider this example:

| task | processing time | priority weight | penalty rate |     |
| ---- | --------------- | --------------- | ------------ | --- |
| A    | 20              | 5               | 10           |     |
| B    | 5               | 4               | 10           |     |
if we were only to consider the total_weight, task A would be executed first, then task B ( 50 > 40). then, the total time would be 
$$20\times 5\times 10 + (20 + 5) \times 4 \times 10 = 2000$$
however, if we were to divide by the processing time before, B would be executed before A (2.5 < 8). so, the total time would be:
$$
5 \times 4 \times 10 + 25 \times 5 \times 10 = 1450
$$
so, we see that dividing it by the processing time would yield a more optimal result. this is because, not only do we want to run the more costly tasks first, we also want to run the one which executes the fastest first.

so, the steps to the solution would be:
- merge the three arrays into one array of 3-tuples
- sort them by their designated weight ($\frac{\text{priority weight} \times \text{penalty weight}} {\text{processing time}}$) in descending order
- keep a running count of elapsed time $r$, and the total cost $c$
- for each task in the sorted list:
	- the cost incurred by the current task is the $(\text{current time} + \text{waited time}) \times \text{weight}$.
	- increment $c$ by that amount
	- increment $r$ by the time taken to complete that task

b. 
Python implementation

```python
def solve(task_names, processing_times, priority_weights, penalty_rates):
	tasks = list(zip(task_names, processing_times, priority_weights, penalty_rates))
	# sort the tasks by the heuristic: priority_weight * penalty_rates / processing_times
	tasks.sort(key=lambda x: (x[2] * x[3]) / x[1], reverse=True)

	total_time = 0
	total_cost = 0
	order = []
	
	for name, t, w, r in tasks:
		# the cost for each task is the sum of the total time waited thus far
		# multiplied by the weight and rate
		cost_for_task = (total_time + t) * w * r
		# increment both items
		total_cost += cost_for_task
		total_time += t
		# then store the order
		order.append(name)
	
	return order, total_cost
```

output for the given task list:

```plaintext
['D', 'B', 'A', 'E', 'C']
1454
```

c. 
if we were to all the priority weights and penalty rates were the same for every task, then our priority heuristic would still ensure that we tackle the functions that take the shortest to execute first (inversely proportional to processing time). that means that we still ensure the correctness of the algorithm even when priority and penalty rate is not included in the heuristic.

## problem 3
a. 
since there are 7 different characters we need to encode in total, the minimum number of bits we can use is 3 bits ($2^{3}=8$)

so, a viable representation of the fixed-length encoding can be just assigning each character to the an increasing binary number:

| character | encoding |     |
| --------- | -------- | --- |
| A         | 000      |     |
| B         | 001      |     |
| C         | 010      |     |
| D         | 011      |     |
| E         | 100      |     |
| F         | 101      |     |
| G         | 110      |     |
|           | 111      |     |


b. 
Huffman tree:
![[Pasted image 20241110152419.png]]
![[Pasted image 20241110152433.png]]
![[Pasted image 20241110152439.png]]
![[Pasted image 20241110152445.png]]
![[Pasted image 20241110152452.png]]
![[Pasted image 20241110152456.png]]
![[Pasted image 20241110152506.png]]

after the chain of removing the minimum value and appending it to the Huffman tree, we yield the encoding of the following:

| character | encoding |     |
| --------- | -------- | --- |
| A         | 0000     |     |
| B         | 100      |     |
| C         | 11       |     |
| D         | 0001     |     |
| E         | 001      |     |
| F         | 01       |     |
| G         | 101      |     |
|           |          |     |
c. 
the total number of bits required is:
$10 \times 4 + 15 \times 3 + 30 \times 2 + 5 \times 4 + 20 \times 3 + 40 \times 2 + 10 \times 3 =335$ bits.

so, the number of bits required to do the fixed-length encoding is:
$(10 + 15+30+5+20+40+10) \times 3 = 390$ bits.

so, the average bits used by Huffman encoding is $\frac{335}{130}=2.57692307692$ bits.

d. 
the compression ratio compared to fixed-length encoding is:
$$\frac{2.576 - 3}{3} \times 100\%=14.10\%$$

## hw5
## problem 1
a.
for each number in the denominations, the number of coins required to make a value of $n$ is dependent on the values before, all the way from $n - d_{1}$ to $n - d_{m}$. this is where the overlapping subproblem comes in, for example, to make coins for 5, 6, and 9 for the denominations 1, 2, and 4 respectively, they all have an overlapping subproblem of 4.

c. 
pseudocode:
```python
def min_coins(denom, n):
	dp = [-1] * (n + 1)
	for i in range(n + 1):
		min_coins = float('inf')
		for d in denom:
			if i - d >= 0:
				min_coins = min(min_coins, dp[i - d] + 1)
		dp[i] = min_coins
	return dp[n]
```

## problem 2
a. 
the entire key with this problem is that, at every given step, we are given 2 choices of how to proceed. we can either: 
- skip (not take) the item, lose out on all the benefit, but conserve some amount of weight
- take the item, and bear the weight.

so, the solution 
```python
dp(i, remaining_weight) = max(
		dp(i + 1, remaining_weight),
		benefit[i] + dp(i + 1, remaining_weight - weight[i]
		)
```

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

prim's
![[Pasted image 20241121005919.png]]
![[Pasted image 20241121010024.png]]
![[Pasted image 20241121010044.png]]
![[Pasted image 20241121010101.png]]
![[Pasted image 20241121010121.png]]
![[Pasted image 20241121010142.png]]
![[Pasted image 20241121010155.png]]
![[Pasted image 20241121010216.png]]

kruskal's: 
![[Pasted image 20241121010435.png]]
![[Pasted image 20241121010500.png]]
![[Pasted image 20241121010524.png]]
![[Pasted image 20241121010548.png]]
![[Pasted image 20241121010618.png]]
![[Pasted image 20241121010646.png]]
![[Pasted image 20241121010740.png]]
![[Pasted image 20241121010804.png]]

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
