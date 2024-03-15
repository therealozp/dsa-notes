in binary trees, we have discussed that using an array representation of binary trees provide much better space optimization, but not so much so for the case of sparser trees where the length of the elements can grow to $2^{h}$, while only $n$ nodes are in use.

a heap is a binary trees storing keys that satisfy the **heap property**: 
- if P is a parent of C, then P $\geq$ C in a max-heap, or P $\leq$ C in a min-heap.
- the last node of a heap is the **rightmost** node, with **maximum depth.**

heaps are usually implemented in fixed and variable-sized arrays (vectors, lists, etc.), which essentially eliminates the need for using linked structures. after a key is inserted, the heap property may be violated, and must be restructured through internal operations.

## implementation with a complete binary tree
the cbt is especially useful for implementing a heap. let $h$ be the height of the heap, then: 
- for $i = 0, \dots, h -1$, there are $2^i$ nodes at depth $i$.
- at depth $h-1$, the internal nodes are all to the left of the external nodes.
- a heap storing $n$ entries will have a height of $\lfloor \log(n) \rfloor$. 