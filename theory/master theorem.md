for any recursive function, we can assume the following method to determine time complexity:

```js
function recur(input x of size n):
	if n < some const k:
		return directly without recursion
	else:
		create `a` number of subproblems, each of size `n/b`
		for each subproblem:
			recur(subproblem)
		combine results
```

then, the time complexity of the recursive algorithm $T(n) = aT\left( \frac{n}{b} \right) + f(n)$
### case 1
if the *number of leaf nodes outweigh the internal calculation complexity*, then the time complexity is dominated by the number of subproblems we have to go through. for example, in the trivial case of calculating the Fibonacci:

```python
def fib(n):
	if n == 0 or n == 1:
		return n
	return fib(n - 1) + fib(n - 2)
```

there is no calculation for determining the subproblems, but jumps straight to solving through subproblems. therefore, the number of leaf nodes will **outweigh** the internal calculation. 

more formally, if $f(n) = O(\log_{b}^{a - \epsilon})$, where $\epsilon > 0$, then
$$T(n) = \Theta(n^{\log_{b}{a}})$$
### case 2
if moving down the tree simplifies the calculation while simultaneously increasing the number of subproblems to solve, or the **sums of internal evaluation costs** at each level are equal, making for $n^{\log_{b}{a}}$ complexity for each problem and a total of $\log_{b}(n)$, then the total time complexity would be
$$
T(n) = \Theta(n^{\log_{b}{a}} \times \log_{b}(n))
$$
for example