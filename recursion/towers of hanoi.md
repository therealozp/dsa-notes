a puzzle where the player has to move all disks of **increasing sizes** from the starting post to the ending post, with one auxiliary post allowed. the disks of larger size must never go on top of one of smaller size. 

the time complexity required to solve this problem is $O(2^n)$, as it takes $2^{n}-1$ steps to move everything over. consequently, the problem requires $2^{n} - 1$ `push()` calls and $2^{n}-1$ `pop()` calls in total. 

the function is implemented as such: 

```cpp
void stacksOfHanoi(int n, stack<int>& stackFrom, stack<int>& stackTo, stack<int>& stackAux) {
	if (n == 1) {  
	// move from top of input stack to output stack
	outputMove(stackFrom.top(), names[&stackFrom], names[&stackTo]);  
	stackTo.push(stackFrom.top()); 
	countPush++;  
	stackFrom.pop(); 
	countPop++;  
	} else {
	// move from input stack to auxiliary stack  
	stacksOfHanoi(n - 1, stackFrom, stackAux, stackTo);  
	// move from input stack to output stack
	outputMove(stackFrom.top(), names[&stackFrom], names[&stackTo]);  
	stackTo.push(stackFrom.top());
	countPush++;  
	stackFrom.pop();
	countPop++;
	// move from auxiliary stack to destination stack  
	stacksOfHanoi(n - 1, stackAux, stackTo, stackFrom);  
	}  
}
```

the `outputMove` function essentially is a function that displays what the current move is. 

the essence of the algorithm is moving everything except the last disk to the auxiliary column, move the last disk to the actual destination, move everything back, and repeat. an example of a call to 3 disks are basically:

![[towers of hanoi 2024-02-13 19.27.04.excalidraw|1000]]

