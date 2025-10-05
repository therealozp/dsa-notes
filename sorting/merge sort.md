#divide-and-conquer #recursion 

one of the basic sorting algorithms based on the divide-and-conquer paradigm. 
- uses a comparator
- $O(n\log(n))$ runtime
- does **not** use an auxiliary priority queue
- accesses data in a sequential manner (which benefits from locality)

## process
on an input sequence `S` with $n$ elements, the algorithm generally consists of three steps: 
- **divide**: partition `S` into two sequences of $\frac{n}{2}$ elements each
- **recur**: recursively solve subproblems with the subset
- **conquer**: merge these two subsequences into a unique sorted sequence.

on a high level: 

```cpp
Algorithm mergeSort(S, C) {  
	if (S.size() > 1) {  
		(S1, S2) = partition(S, n/2)  
		mergeSort(S1, C)  
		mergeSort(S2, C)  
		S = merge(S1, S2)
	}
}
```

```cpp
Algorithm merge(A, B) {  
	R = []
	while (!A.empty() && !B.empty()) {  
		if (A.front() < B.front()) {  
			R.addBack(A.front());
			A.eraseFront();  
		} else {  
			R.addBack(B.front());
			B.eraseFront();  
		}  
	}  
	while (!A.empty()) { 
		R.addBack(A.front()); 
		A.eraseFront(); 
	}  
	while (!B.empty()) { 
		R.addBack(B.front()); 
		B.eraseFront(); 
	}  
	return R  
}
```

## time complexity
merging two sorted sequences, each with $\frac{n}{2}$ elements with a doubly linked list takes $O(n)$ time. since the tree height of the merge sort tree becomes $O(\log(n))$ due to the arrays being halved each time, the total average (and worst-case) runtime of the algorithm is $O(n\log(n))$.