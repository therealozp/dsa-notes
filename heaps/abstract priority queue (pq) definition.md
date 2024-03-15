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

