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

the Master theorem states that: if $T$ is an increasing function that satisfies the recurrence:
 $$T(n) = aT\left( \frac{n}{b} \right) + f(n)$$
for $a \geq 1$ and $b > 1$, and $c = \log_{b}{a}$ then:
- if $f(n) = O(n^{c-\epsilon})$ for some $\epsilon > 0$, then $T(n) = \Theta(n^{c})$
- in $f(n)= \Theta(n^{c})$, then $T(n)=\Theta(n^{c}\log(n))$
- in $f(n) = \Omega(n^{c+\epsilon})$ for some $\epsilon > 0$ and $af\left( \frac{n}{b} \right) < f(n)$, then $T(n)=O(f(n))$

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
this case is best explained with `mergeSort()`, which progressively halves the array and "sorts" by looping over all elements and places them in order. during each function call, the array is split into 2 halves of size $n/2$ each. so, $a = 2, b = 2$. meanwhile, iterating over each elements takes $O(n)$, so in total the function would be:
$$T(n) = 2T\left( \frac{a}{2} \right) + O(n)$$
then, the recursive overhead is $\Theta(n^{\log_{2}2})$, and the iteration overhead is $O(n)$. since they are equal, the complexity is:
$$T(n) = \Theta(n\log(n))$$
### case 3
