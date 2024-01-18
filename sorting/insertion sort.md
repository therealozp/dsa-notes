## idea
1. iterate over the entire array, taking the element at index `i` as the item. 
2. from the immediately previous element, walk backwards; shifting each of the elements larger than key one step up
3. lastly, when the algorithm has found the correct index of the item, then it inserts it with `arr[j + 1] = key`
## time/space complexity
| Category | Best case | Average case | Worst case |
| ---- | ---- | ---- | ---- |
| time complexity | $O(n)$ | $O(n^2)$ | $O(n^2)$ |
| space complexity | $O(n)$ | $O(n)$ | $O(n)$ |
## implementation

```cpp
for (int i = 0; i < arr.length; i++) {
	int item = arr[i];
	int j = i - 1; 
	while (j >= 0 && arr[j] > key) {
		arr[j + 1] = arr[j];
		j -= 1;
	}
	arr[j + 1] = item;
}
```