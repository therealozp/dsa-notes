## in-place
an algorithm is done **in-place** if no auxiliary memory is needed to perform the algorithm.

## stable
a sorting algorithm is **stable** if, for any two entries $(k_{i}, x_{i})$ and $(k_{j}, x_{j})$, such that $k_{i}=k_{j}$ and the $i^{th}$ entry preceeds the $j^{th}$ entry before sorting, then the invariant holds **after** sorting. 

this is important if applications want to preserve the initial ordering of elements using the same key. 

| metric       | quick sort                             | merge sort                             |
| ------------ | -------------------------------------- | -------------------------------------- |
| worst-case   | $O(n^{2})$                             | $O(n\log(n))$                          |
| average-case | $O(n\log(n))$                          | $O(n\log(n))$                          |
| best-case    | $O(n\log(n))$                          | $O(n\log(n))$                          |
| in place     | yes                                    | not traditionally (can be implemented) |
| stable       | not traditionally (can be implemented) | yes                                    |
