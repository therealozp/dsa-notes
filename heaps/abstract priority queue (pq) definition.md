with queues, the order may be basically summarized as first-in-first-out. for pqs, similarly, the object with the highest priority will be popped first from a collection. 

conventionally, we assign a priority to each object. the number 0 represents the highest priority, and decreases with ascending numbers. 

a pq stores a collection of values in a `(priority, value)` pair, and will have the following class interface: 

```cpp
class PriorityQueue {
	public: 
		void insert(e); 
		void removeMin(); 
		Position min(); 
		int size(); 
		bool empty();
}
```

note that the implementation of the pq depends entirely on use cases, it can be removed by max priority or min priority. in the end, the comparator function will determine the order in which elements are removed.

## a naive implementation
a simple (but inefficient) implementation would be using an inserted array. insertion places an element at the back, and `removeMin()` will loop over the entire array searching for the lowest element. like this, `insert()` takes $O(1)$, while `removeMin()` is $O(n)$.

alternatively, by keeping a **sorted** list, the two time complexities of the operations will be reversed.

### the Comparator ADT
this is essentially a class that can derive which elements are lesser/greater than the other. this will be implemented by overloading the operator `()` in C++. 

take the `lesser(p, q)` comparator, for example. from this, we can derive `p == q` by doing `!lesser(p, q) && !lesser(q, p)`, and so on and so forth. an example to compare 2D points: 

```cpp
// left-right comparator  
class LeftRight {  
	public:  
		bool operator()(const Point2D& p, const Point2D& q) const {  
			return p.getX() < q.getX();  
		}  
};  
// bottom-top comparator  
class BottomTop {  
	public:  
		bool operator()(const Point2D& p, const Point2D& q) const {  
			return p.getY() < q.getY();  
		}  
};
```

by utilizing the `std::list<E>`, we can quickly mock up the standard operations of a list-based pq: 

```cpp
template <typename E, typename C> class ListPriorityQueue {  
	public:  
		int size() const; // number of elements  
		bool empty() const; // is the queue empty?  
		void insert(const E& e); // insert element  
		const E& min() const; // minimum element  
		void removeMin(); // remove minimum  
	private:  
		std::list<E> L; // priority queue contents  
		C isLess; // less-than comparator  
}
```

```cpp
ListPQ::insert(const E& e) {
	std::list<E>::iterator p = L.begin(); 
	
	while (isLess(e, *p) && p != L.end()) {
		p++; 
	}
	// insert before position p
	L.insert(p, e)
}
```

since we have kept the list sorted and in-order, the `min()` and `removeMin()` functions can be simply: 

```cpp
E& min() {
	return L.front();
}

void removeMin() {
	L.pop_front(); 
}
```

## sorting with a pq
we can use a priority queue to sort a set of comparable elements, by inserting the elements and calling a number of `removeMin()` operations until we retrieve the correct order. 

```cpp
void PQSort(Sequence S, Comparator C) {  
	P = priority queue with comparator C  
	while (!S.empty()) {  
		e = S.front();  
		S.eraseFront();  
		P.insert(e);  
	}  
	while (!P.empty()) {  
		e = P.min();  
		P.removeMin();  
		S.insertBack(e);  
	}
}
```

## implement a priority queue with a heap
we store a `(key, element)` item at each node of the heap, and always keep track of the position of the last node. 

### insertion
the `insert()` method corresponds to the insertion of a key `k` into the heap, and insertion will consist of 3 steps: 
1. find the new last node (the insertion node `z`)
2. store `k` at `z`
3. restore the heap property (`heapify()`)

### removal
the removal process will only occur when `removeMin()` is called.
1. replace the root with the last node `w`
2. remove `w` (effectively popping the minimum)
3. restore the heap property (`heapifyDown()`)

### the `heapify()` functions
since inserting and removal will eventually lead to the heap property being violated, we must have a function to guarantee the order. 
- `heapifyUp()` will swap the newly-inserted node `k` along an upward path from the insertion node. 
	- the function terminates until `k` reaches root or has a parent with key $\leq$ k.
	- since heap has height $O(\log(n))$, the function has time complexity $O(\log(n))$

```cpp
insert(const E& e) {  
	// add e to heap  
	T.addLast(e);  
	// e's position  
	Position v = T.last();  
	// up-heap bubbling  
	while (!T.isRoot(v)) {  
		Position u = T.parent(v);  
		// if v in order, we're done  
		if (!isLess(*v, *u)) break;  
		// ...else swap with parent  
		T.swap(v, u);  
		v = u;  
	}  
}
```

- `heapifyDown()` will bubble the node replaced with the root `k` downward.
	- the function terminates when `k` reaches leaf or has children with keys $\geq$ k.
	- also runs in $O(\log(n))$

```cpp
  
removeMin() {  
	if (size() == 1) { // only one node? remove it  
		T.removeLast();  
	} else {  
		Position u = T.root();  
		// swap last with root and remove last  
		T.swap(u, T.last()); T.removeLast();  
		// down-heap bubbling  
		while (T.hasLeft(u)) {  
			Position v = T.left(u);  
			if (T.hasRight(u) && isLess(*(T.right(u)), *v)) {  
				v = T.right(u); // v is u's smaller child  
			}  
			// is u out of order? then swap  
			if (isLess(*v, *u)) {  
				T.swap(u, v);  
				u = v;  
			} else { break; } // else we're done  
		}  
	}  
}
```

