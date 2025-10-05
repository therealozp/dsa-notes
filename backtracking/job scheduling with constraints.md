## problem description
given an adjacency matrix of compatibility, e.g. worker $j$ can complete job $i$, determine a solution where all jobs can be completed. otherwise, output no solution.
## state space tree
to effectively prune each step in the tree, we take each level of the tree to be the job we are assigning to. 

each node in the tree represents a combination of worker and job, with the ID $i, j$.

## promising function
we can assign a worker to a certain job if the compatibility matrix says so, and if the job has not been assigned before. 

because of the added bonus of separating the job by level, we can effectively reduce our promising check to only looking at if the worker is compatible.

```python
def promising(job, worker, used_workers): 
	return A[job][worker] == 1
```
## constraint: one worker per job
if this constraint is enforced, we have to add another check in the promising function to see if the worker is used before. then, the promising check will be:

```python
def promising(job, worker, used_workers): 
	# Check compatibility and availability 
	return A[job][worker] == 1 and worker not in used_workers
```

## constraint: each worker has a cost
goal: to minimize the cost while assigning all jobs to different workers. in this case, we need to keep track of more variables, namely the `bestCost` of what we have seen so far, and `currentCost` to keep track of what the current cost is.
### better promising function
instead of solely checking the availability + compatibility, we need to compute the minimum cost. the logic is: $$\text{current cost} + \text{lower bound of remaining tasks} \geq \text{best cost}
$$
if that constraint is ever satisfied, we prune the branch immediately.

```python
def promising(jobIndex, currCost, assigned, bestCost):
	lb = 0
	for j in range(jobIndex, N):
		min_feasible = infty
		for machine in machines:
			if machine is assignable and machine is not assigned:
				min_feasible = min(min_feasible, cost[job][machine])
		if min_feasible = infty:
			return False # no machines found
		lb += min_feasible
		
	if lb + currCost >= bestCost:
		return False
	return True
```

```python
def backtrack(jobIndex, currCost, assigment, assigned):
	if jobIndex == N: # all jobs assigned
		if currCost < bestCost:
			bestCost = currCost
			bestAssignment = assignment
		return
		
	if not promising(jobIndex, currCost, assigned, bestCost):
		return # stop and prune the branch
		
	for each machine:
		if machine is assignable (not infinity) and not assigned:
			assignment[jobIndex] = machine
			assigned[machine] = True
			
			newCost = currentCost + costMatrix[jobIndex][m]
			backtrack(jobIndex + 1, newCost, assignment, assigned)
			
			assigned[machine] = False
```
## time complexity
in the worst case, the we would still have to check all possible solutions, leading to a time complexity of $O(M^N)$, because it is $M$ jobs, with $N$ selections each.

with the minimum weight constraint, we need to check the promising functions as well. so, a valid checking would also cost $O(d)$ more, where $d$ is the remaining depth.
