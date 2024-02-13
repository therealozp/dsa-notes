#theory #arrays
arrays are collections of elements that are of the **same type**, and can divided into 2 sub-classes.

| Actions | Time complexity |
| ---- | ---- |
| insertion at end | $O(1)$ |
| insertion at random position | $O(n)$ |
| element accessing | $O(1)$ |
## static arrays
- defined at compile time
- fixed size
## dynamic arrays
- memory allocated at runtime
- dynamic size, can be allocated later
**Note**: the reallocation of elements are expensive to do, so it is important to optimize it. one solution is to double the length every time it approaches capacity.

## containers
