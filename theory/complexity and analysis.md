analysis measures the efficiency of an algorithm as the input size $n$ into the algo becomes arbitrary large. there can be time and space complexity.
## RAM model
RAM model basically measures how fast an algorithm is by counting the number of instructions required to run something. for example, the code `data[i] < data[j]` requires 3 distinct operations: 
- `data[i]`
- `data[j]`
- `<` to compare

## worst-average-best cases
**worst-case** TC means the maximum number of steps taken for an arbitrary input size $n$. similarly, best cases means the minimum, and average case means average.

however, **it is hard to determine**. so, we opt for a better approach by using **upper and lower bounds**, which Big O notation helps with.
### big O
a technique for abstracting complexity. the key idea is **asymptotic analysis**, which only cares about **how fast it grows** relatively to the input size.
- "fast" algorithms will eventually pass "slow" algorithms after a certain size $n$
- **worst**: $O(g(n))$: for a certain $f(n)$, there exists positive constants $c$ and $n_{0}$ such that $c\times f(n) \leq g(n)$ for all $n \geq n_{0}$.
- **best**: $\Omega(n)$: $f(n) \geq c\times g(n)$
- **average**: $\Theta(n)$: $c_{1}g(n) \leq f(n) \leq c_{2}g(n)$
### properties of order
- $g(n)\in O(f(n)) \iff f(n) \in O(g(n))$
- $g(n)\in \Theta(f(n)) \iff f(n) \in \Theta(g(n))$

$$\Theta(1)\to\Theta(\lg(n))\to\Theta(\sqrt{n })\to \Theta(n) \to \Theta(n\lg(n)) \to \Theta(n^{2)}\to\Theta(n^{j})\to\Theta(a^{k}) \to \Theta(a^{n})\to \Theta(b^{n}) \to \Theta(n!)$$
given that $k > j > 2$ and $b > a > 1$.

## big-O properties
1. reflexivity: $f(n) = O\text{ or }\Omega \text{ or }\Theta(n)$
2. anti-symmetry: $f(n) = O(g(n)) \iff g(n) = \Omega(f(n))$
3. symmetry over $\Theta$
4. transitivity: $f(n) = O(g(n)) \text{ and } g(n) = O(h(n)) \iff f(n) = O(h(n))$
5. envelopment
	- **addition**: $O(f(n)) + O(g(n)) = O(f(n) + g(n))$
	- **multiplication**: $O(f(n)) \times O(g(n)) = O(f(n) \times g(n))$
6. ignore all constant terms (due to asymptotic analysis)
7. only the largest term matters



```cpp
Input: A (array of integers), left (start index), right (end index)  
Output: Index of a peak element  
Algorithm PeakFinder(A, left, right)  
mid = (left + right) // 2  
if (mid == 0 or A[mid] >= A[mid - 1]) and (mid == n - 1 or A[mid] >= A[mid + 1]) then  
return mid  
else if mid > 0 and A[mid - 1] > A[mid] then  
return PeakFinder(A, left, mid - 1)  
else  
return PeakFinder(A, mid + 1, right)
```

base case: for an array of size n = 1, the algorithm correctly identifies the single element (which is the peak element).

induction hypothesis: assume that the array can find the correct peak element in an array of size $k$

we try to prove that the array can correctly identify the peak element in the array of size $k + 1$. 

by the algorithm, we can see that for an array of size $k + 1$, the array is split up into two parts of length `(k + 1) // 2`, which is less than $k$. so, the algorithm is able to find the peak element in one of the two respective arrays, which means it is able to find the peak element in the main array.


2. proof by induction
base case $b_{1} = 3$, which is less than $4$
assume that the inequality holds for $b_{k} \leq 4^{k}$
we try to prove $b_{k + 1} \leq 4^{k + 1}$
$b_{k + 1} = 3\times b_{k} + 2$, and $4^{k+1} = 4\times4^{k}$

then, $b_{k+1} \leq 3\times 4^{k}+ 2$
we want to prove that $3\times 4^{k}+ 2 \leq 4\times4^{k}$
$\Leftrightarrow 2 \le 4^{k}$ which is always true for $k\ge 1$

```cpp
input: n
output: the value for b_n

algorithm B(n):
	if n <= 1: return 3
	else:
		return 3 * B(n - 1) + 2
```

proof for algorithm:
1. **base case**: the first item is correctly computed (3)
2. **inductive hypothesis**: assume that for an arbitrary $k$, the algorithm correctly computes the value for `B(k)`
3. prove for $k + 1$: `B(k + 1) = 3 * B(k) + 2`, and since the algorithm correctly calculates the value for `B(k)`, the correct value of `B(k + 1)` is reached.

use the loop invariant technique to prove Gnome sort.
1. pick a loop invariant: 
> by the **first time** the $k^{th}$ position is reached, the first $k$ elements of the list will be sorted (assuming 0-indexed).
2. **initialization**: for $k$ = 1, the array with one element is trivially sorted.
3. **maintenance**:
	- while in the algorithm, Gnome sort: 
		- steps through the array until it reaches one element that is out of order `A[pos - 1] > A[pos]`
		- moves the element at `pos` backwards in the array, swapping it with the previous element, until the condition is fulfilled and the element is sorted. 
		- then, the pointer moves forward into the next element, until the condition is broken again.
	$\to$ when the $k^{th}$ position is reached for the first time, all previous elements of the array has been sorted.
4. **termination**: because the algorithm terminates when `pos == n`, the loop is guaranteed to terminate correctly.

thus, we have proven the algorithm's correctness.