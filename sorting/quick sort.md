quick sort is a sorting algorithm based on the [[divide-and-conquer]] paradigm.
## process
generally, the fundamental process is similar to merge sort: 
1. **divide**: pick an arbitrary element $x$ to be the **pivot**, and partition S into: 
	- `L`: elements **less** than $x$
	- `E`: elements **equal** to $x$
	- `G`: elements **greater** than $x$
2. **recur**: sort **L** and **G**
3. **conquer**: join **L, E**, and **G**.

## partitioning

```cpp
Algorithm partition(S, p) {  
	L, E, G = empty sequences  
	x = S.erase(p)  
	while (!S.empty()) {  
		y = S.eraseFront()  
		if (y < x) {  
			L.insertBack(y)  
		} else if (y = x) {  
			E.insertBack(y)  
		} else { // y > x  
			G.insertBack(y)  
		}  
	}  
	return L, E, G  
}
```

**remove**, in turn, each element `y` from the input sequence `S`. then, we can insert `y` into their respective destination arrays (**L, E, G**), depending on the result of the comparison with the pivot $x$.

thus, for $n$ elements, the partitioning steps takes $O(n)$ time.

## implementation

```cpp
template <typename E, typename C>
void quickSort(std::vector<E>& S, const C& less) {  
	if (S.size() <= 1) return; // already sorted  
	quickSortStep(S, 0, S.size()-1, less); // call sort utility  
}
```

```cpp
template <typename E, typename C>  
void quickSortStep(std::vector<E>& S, int a, int b, const C&  
less) {  
	if (a >= b) return; // 0 or 1 left? done  
	E pivot = S[b]; // select last as pivot  
	int l = a; // left edge  
	int r = b - 1; // right edge  
	// until indices cross  
	while (l <= r) {  
		// scan right till larger then scan left till smaller  
		while (l <= r && !less(pivot, S[l])) l++;  
		while (r >= l && !less(S[r], pivot)) r--;  
		if (l < r) { 
			std::swap(S[l], S[r]); 
		} // both elements found  
	}  
	std::swap(S[l], S[b]); // store pivot at l  
	quickSortStep(S, a, l - 1, less); // recur on both sides  
	quickSortStep(S, l + 1, b, less); // recur on both sides
}
```

the core to this implementation lies in the strategic swapping of the elements. first, we initialize two left/right pointers, and set them to the current boundaries of the elements. 

the outermost `while` loop will **NOT** stop until two indices have crossed, meaning the partitioning is done **in-place**. to illustrate, consider the example array: 
- `[85, 24, 63, 45, 17, 31, 96, 50]`, pivot = 50, `l` = 85, `r` = 96. the first while terminates, and the second while iterates until r = 31. these two elements are swapped.
- `[31, 24, 63, 45, 17, 85, 96, 50]`. the first loop iterates until l = 63, and the second loop until r = 17. these two elements are swapped.
- `[31, 24, 17, 45, 63, 85, 96, 50]`. now, l iterates until l = 63, so the outermost loop terminates. then, l and pivot are swapped.
- `[31, 24, 17, 45, 50, 85, 96, 63]`. we are left with two partitions: `[31, 24, 17, 45]`, and `[85, 96, 69]`. this corresponds with the partitioning algorithm we wanted.

## choice of pivot
the choice of pivot heavily impacts the runtime of the algorithm. the above implementation where the last element is always chosen will result in $O(n^{2})$ runtime on already sorted arrays and array of duplicates. 

therefore, the worse case for quick sort occurs when the pivot consistently falls to the minimum/maximum. then: 
- one of the partitions is size 0, while the other is $n - 1$.
- the runtime is proportional to sum $n + (n - 1) + (n -2) + \dots + 2 + 1$, or $O(n^2)$.

a better strategy would be choosing a **random element** or picking the **middle index** of the partition to make it **average** $O(n\log(n))$.