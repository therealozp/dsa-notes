## definition
any recursive function should satisfy two requirements: 
- it has a **base case**
- each successive call to recurse should **move towards the base case.**

for example, to sum up all elements in a list, instead of calling a linear sum, we can do a sort of `binarySum`:

```cpp
int binarySum(int [] A, int i, int n) {
	// summing n integers in array starting at index i
	if (n == 1) {
		return A[i];
	}

	return binarySum(A, i, ceil(n / 2)) + binarySum(A, i + ceil(n / 2), floor(n / 2));
}
```

take an array `[1, 2, 3, 4, 6, 8, 0]`. then, calling `binarySum(A, 0, 7)` makes successive calls to `binarySum(A, 0, 4)` and `binarySum(A, 4, 3)`, which calls other functions respectively until every of those calls reach a base case of `n == 1`.