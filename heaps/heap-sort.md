consider a pq with $n$ items implemented with a heap.
- uses $O(n)$ space
- `insert()` and `removeMin()` takes  $O(\log(n))$
- `size()`, `empty()`, `min()` takes $O(1)$

using a heap-based pq, we can sort a sequence of $n$ elements in $O(n\log(n))$ time, called heap-sort. this is much better than the quadratic sorting algorithms like insertion and selection sort.

## implementation
we represent a heap with $n$ keys with a vector of length $n + 1$. for any node at index $i$, the **left child** is at $2i$ and the **right child** is at $2i + 1$.
- we don't have to store the links between two elements
- index 0 is not used
- `insert()` means inserting at index $n + 1$
- `removeMin()` removes at $n$

then, considering that we are using a vector to represent a heap, we can simply sort the entire vector in-place, by building a max/min heap.

suppose that we have a vector `[6, 3, 4, 1, 5, 2]`.
1. we `heapify()` the vector: `[6, 4, 5, 2, 3, 1]`
2. we `removeMax()`, swapping with the last, and shrink the heap's size down by 1. this way, we effectively create 2 partitions of one sorted portion, and another unsorted portion. `[1, 4, 5, 2, 3 | 6]`
3. we `heapify()` the vector again: `[5, 3, 4, 2, 1 | 6]`
4. and repeat step 2: `[1, 3, 4, 2 | 5, 6]`

the process repeats until the heap is empty, and the entire vector is fully sorted.