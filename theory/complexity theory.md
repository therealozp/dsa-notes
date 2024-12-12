## tractability
a certain problem is **tractable** if there exists a polynomial that solves it. in the worst case, the growth rate is always bounded by a polynomial.

as a refresher, on the hierarchy of complexity, the following functions are polynomial: 
$$\begin{equation}
\begin{split}
\Theta(1)\to\Theta(\lg(n))\to\Theta(\sqrt{n })\to \Theta(n) \to \Theta(n\lg(n)) \to
 \Theta(n^{2)} \to\Theta(n^{j}) \to \Theta(n^k)
\end{split}
\end{equation}$$
by contrast, intractable problems are reserved for when the solution will take a minimum exponential time. this is usually due to **some intrinsic part of the problem**, not the algorithm.

### complexity categories
1. problems where polynomial-time algorithms exist
2. problems definitively proven to be impossible (intractable), such as Alan Turing's halting problem
3. problems where it is not proven to be impossible, but a polynomial algorithm has also not been found either.

surprisingly, most of the problems in the realm falls into the third category.
### polynomial-time
- sorting takes $O(n\log n)$
- searching in sorted array takes $O(\log n)$
- matrix multiplication takes $O(n^3)$

there are algorithms that have stupidly time-complex brute-force solution, but the actual optimized solutions are polynomial, such as:
- shortest path: $O(n!)$ with brute-force, $O(n^2)$ for Dijkstra's, where $n$ is number of nodes (vertices) inside the graph
- optimal binary search tree: construct a BST that minimizes total search cost, given a set of keys and probabilities of access
	- brute-force $O(n!)$
	- dynamic programming $O(V^3)$
- finding the MST
	- brute-force: $O(2^E)$, where $E$ is the number of edges as we are finding the number of subsets of edges.
	- using Prim's or Kruskal's reduce this to a polynominal TC.

### proven intractable
- unrealistic definition: finding all Hamiltonian cycles, which are paths that visit each vertex once. we call this intractable because if there was an edge from every other edge, there would be $(n-1)!$ circuits; and outputting all these circuits while remaining polynomial is impossible.
- undecidable problems: proven that algorithms to solve them could not exist. e.g. halting problem
- decidable intractable: problems proven from automata and mathematical logic intractable.

### not proven to be intractable
- TSP: $O(2^{n}\times n^2)$
- graph coloring: $O(2^{n}\times n)$
- sum of subsets: $O(2^n)$
- 0-1 knapsack $O(2^n)$
	- with DP: $O(nW)$, which is a pseudo polynomial.

## NP completeness
**decision problems** are problem where the output just a "true" or a "false" ("yes" or "no")

more complex problems can be restricted to become decision problems, which helps us find the **lower bound** for complexity of that harder problem.

if a polynomial time algorithm for the harder problem is found, a polynomial time algorithm for the easier decision problem can also be found. for example:
- TSP:
	- originally, to find a tour with minimum edge weight
	- decision problem: for a given positive number $d$, is there a tour with length $\leq d$?
- 0-1 knapsack:
	- find maximum profit to be placed inside a knapsack with limited capacity
	- decision: for a given profit $P$, is it possible to load the knapsack with that amount of profit such that total weight does not exceed capacity?
- graph coloring:
	- find minimum number of colors needed to color a graph so that no two adjacent vertices are the colored the same.
	- decision: for a number of colors $m$, is it possible to use that to solve that problem?

### P-class problems
the set of all problems that can be solved by polynomial time algorithms, such as **decision version of problems** such as searching, shortest path, MST.

the problematic thing with NP-completeness is that we cannot prove that something does not belong to P, unless it is completely proven that **it is impossible** to develop polynomial time algorithms for such a problem.

#### non-deterministic algorithms
consists of two main phases: non-deterministic (guessing) and deterministic (verifying).

1. non-deterministic phase means continually guessing the output, because there is no step-by-step instruction. instead, the computer guesses at random.
2. verify: check if the guess is feasible.

### NP-class problem
set of all problems that, when given a solution, is able to be verified in polynomial time. by relation, we say that $P\in NP$.

all problems mentioned above (TSP, 0-1 knapsack, and graph coloring) are all NP-class problems. 

![[Pasted image 20241212102523.png]]

### NP-complete
NP-complete is the set of problems where:
- efficient algorithms has never been found
- never proven that one is impossible

NP-complete problems belongs to both NP and NP-hard.

## reduction
polynomial reduction from X to Y (X is reducible to Y) is when:
- there exists an algorithm $f$ that transforms instances $x \in X$, to instances $y =f(x) \in Y$
- $x$ = yes $\iff$ $f(x)$ = yes.

reductions do not have to be symmetrical, so if $X$ is reducible to $Y$, that doesn't mean $Y$ is reducible to $X$. instead, a better way to understand it is the reduced problem $Y$ is at least as hard as $X$. 

if $X \leq_p Y$, it means that $X$ can be solved using an algorithm for $Y$ plus a polynomial amount of extra work. so, if we can reduce a known hard problem (e.g., SAT) to another problem (e.g., 3-SAT), you show that the second problem is at least as hard as the first.

## NP-hard
NP-hard describes a set of problems $H$ where:
- every polynomial problem in $NP$ can be reduced in polynomial time to $H$
- $H$ does not have to be in $NP$, nor does it have to be a decision problem.

finding a polynomial time algorithm to solve any NP-hard problem gives us **P-time** algorithms for **all problems in NP**. by association, every NP complete problem belongs to NP-hard.

![[Pasted image 20241212104723.png]]