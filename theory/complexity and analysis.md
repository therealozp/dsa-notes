analysis measures the efficiency of an algorithm as the input size $n$ into the algo becomes arbitrary large. there can be time and space complexity.
## RAM model
RAM model basically measures how fast an algorithm is by counting the number of instructions required to run something. for example, the code `data[i] < data[j]` requires 3 distinct operations: 
- `data[i]`
- `data[j]`
- `<` to compare

in more formal terms, the RAM model is an abstract machine where 
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
1. reflexivity: $f(n) = O\text{ or }\Omega \text{ or }\Theta(f(n))$
2. anti-symmetry: $f(n) = O(g(n)) \iff g(n) = \Omega(f(n))$
3. symmetry over $\Theta$
4. transitivity: $f(n) = O(g(n)) \text{ and } g(n) = O(h(n)) \iff f(n) = O(h(n))$
5. envelopment
	- **addition**: $O(f(n)) + O(g(n)) = O(f(n) + g(n))$
	- **multiplication**: $O(f(n)) \times O(g(n)) = O(f(n) \times g(n))$
6. ignore all constant terms (due to asymptotic analysis)
7. only the largest term matters