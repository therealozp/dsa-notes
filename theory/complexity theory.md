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

### polynomial-time
- sorting takes $O(n\log n)$
- searching in sorted array takes $O(\log n)$
- matrix multiplication takes $O(n^3)$

there are algorithms that have stupidly time-complex brute-force solution, but the actual optimized solutions are polynomial, such as:
- shortest path: $O(n!)$ with brute-force, $O(n^2)$ for Dijkstra's, where $n$ is number of codes
- 