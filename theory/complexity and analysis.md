analysis measures the efficiency of an algorithm as the input size $n$ into the algo becomes arbitrary large. there can be time and space complexity.
## affecting factors
- hardware and system specs (processor speed, RAM, cache, ...)
- implementation details (coding languages, compiler)
- number of instructions
- external factors, such as other running processes, system load, etc.
## RAM model
RAM model basically measures how fast an algorithm is by counting the number of instructions required to run something. for example, the code `data[i] < data[j]` requires 3 distinct operations: 
- `data[i]`
- `data[j]`
- `<` to compare

formally, the RAM model of computation is an abstract machine used to simplify analysis of algorithms, by assuming that the algorithm has access to infinite memory. 

the main assumptions of the RAM model are: 
- all basic functions (assignment, arithmetic, branching, etc.) take 1 operation (except for loops and custom-defined functions)
- memory access is instantaneous
- we have access to unlimited memory

advantages:
- it simplifies analysis of algorithms
- provides a uniform base for computing
- it closely mirrors actual computer architecture
## worst-average-best cases
**worst-case** TC means the maximum number of steps taken for an arbitrary input size $n$. similarly, best cases means the minimum, and average case means average.

however, **it is hard to determine**. so, we opt for a better approach by using **upper and lower bounds**, which Big O notation helps with.
### big O
a technique for abstracting complexity. the key idea is **asymptotic analysis**, which only cares about **how fast it grows** relatively to the input size.
- "fast" algorithms will eventually pass "slow" algorithms after a certain size $n$
- used for practical analysis and worst-case estimations.
### notation
**upper bound (at most)**: $O(g(n))$: for a certain $f(n)$, there exists positive constants $c$ and $n_{0}$ such that $c\times f(n) \leq g(n)$ for all $n \geq n_{0}$.
**lower bound (at least)**: $\Omega(n)$: $f(n) \geq c\times g(n)$
**upper and lower bound (same rate as)**: $\Theta(n)$: $c_{1}g(n) \leq f(n) \leq c_{2}g(n)$

to simplify understanding, when we say $f(n) = O(g(n))$, that effectively means $f(n) \text{ is upper bounded by } g(n)$. similarly to other symbols as well.

### properties of order
- $g(n)\in O(f(n)) \iff f(n) \in \Omega(g(n))$
- $g(n)\in \Theta(f(n)) \iff f(n) \in \Theta(g(n))$
- for any $a, b > 1$, $\log_{a}{n} \in \Theta(\log_{b}{n})$. this implies all logarithmic function will be contained in the same complexity category, which can be simplified down to $\Theta(\lg n)$.
- if $b > a> 0$, then $a^{n}\in o(b^{n})$. so, they are not in the same complexity category.

$$
\begin{equation}
\begin{split}
\Theta(1)\to\Theta(\lg(n))\to\Theta(\sqrt{n })\to \Theta(n) \to \Theta(n\lg(n)) \to
\\
 \Theta(n^{2)} \to\Theta(n^{j})\to\Theta(a^{k}) \to \Theta(a^{n})\to \Theta(b^{n}) \to \Theta(n!)
\end{split}
\end{equation}
$$
given that $k > j > 2$ and $b > a > 1$.

### properties of logs
- all logs are scalar multiples of one another. this means property 7 of big O can be applied.
- $\lg(ab) = \lg(a) + \lg(b)$
- $\lg(\frac{a}{b}) = \lg(a) - \lg(b)$
- $\lg(a^b) = b\lg(a)$ - helpful when dealing with $\lg(a^{n)}= n\lg(a)$
$$
\sum_{i = 1}^{n}{\frac{1}{i}} = \Theta(\lg(n))
$$

## big-O properties
1. reflexivity: $f(n) = O\text{ or }\Omega \text{ or }\Theta(f(n))$
2. anti-symmetry: $f(n) = O(g(n)) \iff g(n) = \Omega(f(n))$
3. symmetry over $\Theta$
4. transitivity: 
	- $f(n) = O(g(n)) \text{ and } g(n) = O(h(n)) \iff f(n) = O(h(n))$
	- similar interaction for $\Omega$ and $\Theta$
5. envelopment similar interaction for $\Omega$ and $\Theta$
	- **addition**: $O(f(n)) + O(g(n)) = O(f(n) + g(n))$ 
	- **multiplication**: $O(f(n)) \times O(g(n)) = O(f(n) \times g(n))$
6. ignore all constant terms (due to asymptotic analysis)
7. only the largest term matters

## little O
mostly used int theoretical contexts to emphasize strict growth rate differences (when demonstrating proofs)

$f(n) = o(g(n))$ if, **for every positive real constant** $c$, there exists a *nonnegative* integer $N$ such that:
$$\forall n \ge N, f(n) \le c\times g(n)$$
for big O, we only need to find **some** positive real constant. for little o, it most hold for every single constant.

same thing for little $\Omega$ ($\omega$). if $f(n) = \omega(g(n))$, $f(n) \ge c\times g(n)$ for all constants of $c$.


## how to analyze algorithms
- except for loops and function calls, everything is $\Theta(1)$.
- loops:
	- number of iterations?
		-  if it is **incrementing** by $c$ `+= c`: divide range by $c$ to get num iterations.
		- it it is **scaling** by $c$ `*= c`: take the $\log_{c}$ of end/start ratio.
	- estimate loop body runtime
	- does loop runtime depend on iteration number (e.g. nested loops)?
		- **no**: `total = iterations * time per iteration`
		- **yes**: `total = sum of ALL iterations`
- overall complexity: sum of complexities (dominated by largest)