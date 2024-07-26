#heap #theory
in binary trees, we have discussed that using an array representation of binary trees provide much better space optimization, but not so much so for the case of sparser trees where the length of the elements can grow to $2^{h}$, while only $n$ nodes are in use.

a heap is a binary trees storing keys that satisfy the **heap property**: 
- if P is a parent of C, then P $\geq$ C in a max-heap, or P $\leq$ C in a min-heap.
- the last node of a heap is the **rightmost** node, with **maximum depth.**

heaps are usually implemented in fixed and variable-sized arrays (vectors, lists, etc.), which essentially eliminates the need for using linked structures. after a key is inserted, the heap property may be violated, and must be restructured through internal operations.

## implementation with a complete binary tree
the cbt is especially useful for implementing a heap. let $h$ be the height of the heap, then: 
- for $i = 0, \dots, h -1$, there are $2^i$ nodes at depth $i$.
- at depth $h-1$, the internal nodes are all to the left of the external nodes.
- a heap storing $n$ entries will have a height of $\lfloor \log(n) \rfloor$.

from then, we can derive a basic interface: 

```cpp
template <typename E> class CompleteTree {  
	public:  
		class Position; // node position type  
		int size() const; // number of elements  
		
		Position left(const Position& p); 
		Position right(const Position& p); 
		Position parent(const Position& p); 
		  
		bool hasLeft(const Position& p) const; // node left child?  
		bool hasRight(const Position& p) const; // node right child?
		  
		bool isRoot(const Position& p) const; // is this the root?  
		Position root(); // get root position  
		Position last(); // get last node
		  
		void addLast(const E& e); // add a new last node  
		void removeLast(); // remove last node  
		// swap node contents  
		void swap(const Position& p, const Position& q);  
};
```

we can further expand this by using `std::vector` (note that this implementation's vector starts at index 1): 

```cpp
template <typename E> class VectorCompleteTree {
	private:  
		std::vector<E> V; // tree contents
		  
	protected:  
		// map an index to a position  
		Position pos(int i) { return V.begin() + i; }  
		// map a position to an index  
		int idx(const Position& p) const { return p - V.begin(); }
		  
	public:  
		VectorCompleteTree() : V(1) {}  
		typedef typename std::vector<E>::iterator Position;
		
		int size() const { return V.size() - 1; }
		  
		Position root() { return pos(1); }  
		Position last() { return pos(size()); }
		  
		void addLast(const E& e) { V.push_back(e); }  
		void removeLast() { V.pop_back(); }
		  
		void swap(const Position& p, const Position& q) {  
			E e = *q;  
			*q = *p;  
			*p = e;  
		}  
		 
		Position left(const Position& p) { 
			return pos(2 * idx(p)); 
		}  
		Position right(const Position& p) { 
			return pos(2 * idx(p) + 1); 
		}  
		Position parent(const Position& p) { 
			return pos(idx(p)/2); 
		}  
		bool hasLeft(const Position& p) const { 
			return 2 * idx(p) <= size(); 
		}  
		bool hasRight(const Position& p) const {
			return 2 * idx(p) + 1 <= size(); 
		}  
		bool isRoot(const Position& p) const { 
			return idx(p) == 1; 
		}
};
```

