AVL trees are a subset of binary search trees, but they always maintain a balance. It is a BST such that for **every internal node** `v` of `T`, the heights of their children can differ by **at most 1.**

to implement this, AVL trees have a `restructure()` method, which are split into the following 4 cases:
## case A: single, left rotation
when the AVL tree is right-heavy, we can perform this rotate left operation: 
1. replace the subtree rooted at `a` with a new subtree rooted at `b`
2. make `a` the left child of `b`, move `t1` to be `a`'s right subtree
3. `c` is still the right child of `b`, and the order remains unchanged.

![[AVL trees 2024-03-15 23.40.31.excalidraw|1000]]

## case B: single, right rotation
when the AVL tree is left-heavy, the the rotate right operation can be done. suppose that the subtree is rooted at `c`. 
1. replace subtree rooted at `c` with a new subtree rooted at its left child, `b`.
2. we can make the right subtree of `b` the **left subtree** of `c`.
3. `a` is still the left child of `b`, and the order still remains unchanged.

![[AVL trees 2024-03-16 00.06.51.excalidraw|1000]]

## case C: double, right-left rotation
there are 2 sub-steps to this rotation: first, rotate right on the right subtree, and rotate left on the root.

1. do a right rotation on the right subtree: replace subtree rooted at `c` with a subtree rooted at `b`. then, the **right subtree of b** becomes the **left subtree of c**, and `c` becomes `b`'s **right** subtree.
2. from there, we get a normal case A, which we can perform by doing the rotation steps.

![[AVL trees 2024-03-16 00.22.02.excalidraw|1000]]

## case D: double, left-right rotation
like case C, there this also involves the tree maintaining its BST invariant when there is a special case involving 2 differently-branching paths. 

1. do a left rotation on the left subtree: replace subtree rooted at `a` with a subtree rooted at `b`. then, the **left subtree of b** becomes the **right subtree of c**, and `a` becomes `b`'s **left** subtree.
2. from there, we get a normal case B, which we can perform by going over its rotation steps.

![[AVL trees 2024-03-16 00.32.01.excalidraw|1000]]

## a general algorithm
from the above 4 cases, we can derive a more general approach that is guaranteed to always work for the 4 cases. to do this, we suppose that: 

```cpp
Algorithm retstructure(x) {
	Node x; 
	Node y; // parent
	Node z; // grandparent

	1. let a, b, c be an in-order listing of the nodes x, y, and z
	2. let t0, t1, t2, t3 be an in-order listing of the subtrees of x, y, and z (not containing these nodes)
	3. replace the subtree at z with the subtree rooted at b
	4. let a be the left child of b, make t0 and t1 be the left and right subtrees of a
	5. let c be the right child of b, make t2 and t3 the left and right
		subtrees of c
}
```

## AVL insertion
we follow the same insertion procedure as in the binary search tree, always done by expanding an external node into the right area. however, when the tree is unbalanced, we will have to call the balance operation using the general algorithm above. 

## AVL removal
similarly, removal acts the same as in a BST, where the node removed will be come an external node. the balance process will be the same nonetheless.
