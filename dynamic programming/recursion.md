## definition
any recursive function should satisfy two requirements: 
- it has a **base case**
- each successive call should **move towards the base case.**

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

## faster powers
by using binary powering, which increases the exponent of a base twice per call, we effectively make powering calls from $O(n)$ to $O(\log(n))$. implementation looks like this: 

```cpp
long power2(int x, int n) {
	if (n == 1) return x; 
	else if (n == 0) return 1; 

	long t = power2(x, n / 2); 
	if (n % 2 == 0) return t * t; 
	else return x * t * t;
}
```

the initial call can look like `power2(5, 11)`. then, it makes subsequent calls to find t, first by reducing the exponent to 5, then 2, then 1. the recursive calls subsequently return $x$ after the last call, and since 2 is even, it returns $x^{2}$. then, it returns, and since 5 is odd, it returns $x^{2} \times x^{2}\times x$. the process repeats until we get the desired result.