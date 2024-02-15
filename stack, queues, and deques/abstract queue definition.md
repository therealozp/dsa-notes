#theory #queue
a queue is first-in-first-out (FIFO) data structure. most common applications are client-server models, grocery stores, SFTP Clients, etc.

at its most basic level, a queue ADT should support operations such as `enqueue()`, `dequeue()`, `empty()`, and `front()`. of course, for variants such as a double-ended queue (deque), we need other auxiliary functions as well. 

## implementations
since all queue elements should perform in $\Theta(1)$ time, we will employ two other data structures in its implementation: 
- singly-linked lists
- circular arrays
### array queue
although an array queue is a viable option, it does not allow all operations to run in $O(1)$ time. however, we can still make this work if we maintain `front` and `rear` pointers in the array, and use them in a circular fashion. 

```cpp
template <typename E>  
class ArrayQueue {  
	unsigned int DEF_CAPACITY = 100; // default queue capacity  
	public:  
		ArrayQueue(int cap = DEF_CAPACITY); // constructor  
		int size() const; 
		bool empty() const;
		const E& front() const throw(QueueEmpty); // the front element  
		void enqueue(const E& e) throw(QueueFull); // enqueue at rear  
		void dequeue() throw(QueueEmpty); // dequeue at front  
	private: 
		E* Q; // array of queue elements  
		int capacity; // queue capacity  
		int front;
		int rear;   
		int n; // number of elements  
};
```

apart from implementing the trivial functions such as `empty()`, `size()`, we can work out the circular aspect of this as follows: 

```cpp
template <typename E>  
void ArrayQueue<E>::enqueue(const E& e) throw(QueueFull) {  
	if (size() == capacity) {  
		throw QueueFull("enqueue to full queue");  
	}  
	Q[rear] = e;  
	rear = (rear + 1) % n;  
	n++;  
}

// dequeue element at front  
template <typename E>  
void ArrayQueue<E>::dequeue() throw(QueueFull) {  
	if (empty()) {  
		throw QueueEmpty("Dequeue from empty queue");  
	}  
	front = (front + 1) % n;  
	n--;  
}
```

this is done by modifying the `front` and `rear` pointers to step ahead by 1. the beauty of this implementation lies in the fact that we don't ever have to worry about the actual position of the elements, so long as we have the two pointers, it will be enough to handle any `enqueue()` or `dequeue()` operations we might eventually have. 
### CLL queue
alternatively, with the circular linked list [[implementations]], we have the `advance()` method to move the cursor (the rear), and the `front()` method that does the `dequeue()`. 
## deque
deques will have to implement an extra 2 functions to make up the complete suite of `insertFront()`, `insertBack()`, `removeFront()`, and `popBack()`. this one is best implemented using a DLL, but there are many alternatives to it. 