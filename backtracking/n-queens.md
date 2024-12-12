n-queens problem is a problem that can be solved nicely by backtracking. however, a key note is that this will get slow very quickly as the number of queens on the board scales.

the intuition behind n-queens is simple: we place a queen on each column, positioning it in such a way that it is not attacked by any other queen on the board. this intuition lends itself very nicely to a backtracking solution, where each level in the decision tree shows where it can be put.

![[Pasted image 20241211224456.png]]

at every step, there are $n$ possible slots we can insert the queen in. that is why the naive algorithm has an absurdly high time complexity of $O(n^n)$. 

however, if backtracking is applied, instead of having $n$ paths to explore at every level, the number of paths shrink as the number of levels go on. so, the number of steps to explore is bounded by $n$, then $n-1$, then $n-2$, and so on until 1 is left. so, backtracking narrows runtime down to $O(n!)$

that means we have identified the correct decision tree, where each path node $(i, j)$ represents a location on the board. now, how do we define the promising function, e.g. if the queens can be placed on the board so that no one is being attacked?
### promising function
the condition for two queens to attack each other is if they are:
- same row
- same column
- same diagonal

since we are incrementing the role on each iteration, or the level of traversal, it can be accounted for by default. that leaves us with checking the column and the same diagonal.

note, that on the same diagonal, the slope is always 1 or -1. so, using the slope formula, we can derive the equation:
$$\| \frac{x_{1}-x_{2}}{y_{1}-y_{2}}\| = 1$$
which leads to: 
$$\|x_{1}-x_{2}\|=\|y_{1}-y_{2}\|$$

```python
def isValidPosition(i, j):
	for di, dj in queens:
		# assuming queens hold the list of queens up to this point
		if i == di or j == dj or abs(i - di) == abs(j - dj):
			return False
	return True
```

```python
n = board_size
def solve_nQueens(i, queens = [], solutions = []):
	if i == n:
		# solution has been reached
		# do something here
		solutions.append(queens.copy())
		return
	for j in range(n):
		if isValidPosition(i, j):
			queens.append((i, j))
			solve_nQueens(i + 1)
			queens.pop() # backtrack
```

