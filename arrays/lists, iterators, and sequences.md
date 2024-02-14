#theory
other than using **indexing** to reach elements in a linear structure, we can also implement a **list ADT** and use a **position** element that abstracts the notion of "place" in a list. 

in essence, the list ADT uses the linked list under the hood alongside another ADT that abstracts "place". however, this approach means that indexing will take **linear time** instead of constant as in arrays.

## position ADT
the position ADT basically gives a unified view of how we view "place" in a data structure, such as a cell in an array or a node in a linked list. 

the only method that this ADT should need is: 

```cpp
template <typename T>
// returns the reference to the element stored at this position
T p.element();
// or, we can also overload the * operator: 
T *p; 
```

## containers and iterators
an **iterator** abstracts the scanning over the data structure to search for an element:
- `*p` should return the element referenced by this iterator
- `p++` or `++p` should **advance** the iterator

a **container** is an ADT that supports element access using iterators (operations such as `store` or `access`): 
- `begin()`: returns an **iterator** to the first element
- `end()`: returns an **imaginary iterator** to the position ***after*** the last element. 

there are many variants of iterators, and ranging from least to most powerful: 
- **const iterator**: only allows read access
- **standard iterator**: allows read and write access
- **bidirectional iterator**: allows `++` and `--` operations to move iterator
- **random access iterator**: allows for `p + i` and `p - i`, essentially behaves like a pointer

## implementation: deque
we have discussed the [deque](abstract%20queue%20definition.md), and using that as a base, we add on the iterator datatype to loop through the structure. intrinsically, it should have the following members: 

```cpp
template <typename T> class Deque<T>: 
	private: 
		// iterators
		Deque::Iterator begin(); 
		Deque::Iterator end(); 

	public: 
		// update methods
		void insert_front(T); 
		void insert_back(T); 
		T pop_back(); 
		T push_back(); 
		
		// iterator-based updates
		void insert(Deque::Iterator p, T e); 
		void remove(Deque::Iterator p); 
```

then, the iterator class definition should be as follows: 

```cpp
class Iterator {
	public:  
		T& operator*() {
			return v->elem;
		}; 
		
		// compare positions
		bool operator==(const Iterator& p) const {
			return v == p.v;
		};
		  
		bool operator!=(const Iterator& p) const {
			return v != p.v; 
		};  
		
		// move to next position 
		Iterator& operator++() {
			v = v->next; 
			return *this;
		};  
		
		// move to previous position  
		Iterator& operator--() {
			v = v->prev; 
			return *this
		}; 
		friend class NodeList; // give NodeList access  
		
	private:  
		Node* v; // pointer to the node  
		// private constractor create from node
		Iterator(Node* u); {
			v = u; 
		}
```

within this definition: 
- the first function overloads the `*` operator, returning the **address** of the element. 
- the second and third functions implement a comparison check, taking in another address of the iterator to compare.
- the fourth and fifth functions defines operators to move forwards and backwards, of course returning an address operator (because the `++` operation essentially means `p = p + 1`) 
- the last line gives only the `NodeList` class access to the element.

inside the double-ended queue, the iterator will have its use as the `begin()` and `end()` positions for iteration. on top of the usual deque declaration, we also initialize the two following members: 

```cpp
// NodeList and Deque are conceptually equivalent
class NodeList {
	public: 
		// default operations like constructor, destructor, push_front(), 
		// push_back(), pop_front/back(), etc.
		
		Iterator begin() {
			// returns an iterator to the first element
			return Iterator(header->next); 
		}; 
		Iterator end(
			// returns an iterator the point beyond last element
			return Iterator(trailer->next); 
		); 

	private:
		int n; // number of items
		Node* header; 
		Node* trailer; 
}
```

to use the private constructor in the `Iterator` class, we can use the following: 

```cpp
NodeList::Iterator NodeList::begin() {
	return Iterator(header->next); 
}

NodeList::Iterator NodeList::end() {
	return Iterator(trailer);
}
```

### insert / erase operations with iterators
since we now have the `Iterator` class returning everything we want to, we can call `insert()`, `pushfront()` and `push_back()` much more succinctly. 

```cpp
// insert e before p  
void NodeList::insert(const NodeList::Iterator& p, const Elem& e) {  
	Node* w = p.v; // p's node  
	Node* u = w->prev; // p's predecessor  
	Node* v = new Node; // new node to insert  
	v->elem = e;  
	v->next = w; w->prev = v; // link in v before w  
	v->prev = u; u->next = v; // link in v after u  
	n++;  
}  
// insert at front  
void NodeList::insertFront(const Elem& e) {  
	insert(begin(), e);  
}  
// insert at rear  
void NodeList::insertBack(const Elem& e) {  
	insert(end(), e);  
}  
```

similarly, the `erase` function family can also be much more well-defined: 

```cpp
// remove p  
void NodeList::erase(const Iterator& p) {  
	Node* v = p.v; // node to remove  
	Node* w = v->next; // successor  
	Node* u = v->prev; // predecessor  
	u->next = w; w->prev = u; // unlink p  
	delete v; // delete this node  
	n--; // one fewer element  
}  
// remove first  
void NodeList::eraseFront() {  
	erase(begin());  
}  
// remove last  
void NodeList::eraseBack() {  
	erase(--end());  
}  
```

### time complexity
- the space used is always linear $O(n)$
- the space used by each position is constant $O(1)$
- all operations of the list ADT (because there isn't a traversal method) runs in constant time $O(1)$
- the `*` operator (`.element()`) also runs in $O(1)$ time.

| array-based | linked list-based |
| ---- | ---- |
| array `A` of $n$ elements | doubly-linked list `L`, storing elements between a header an a trailer node |
| index `i` to keep track of the cursor | pointer to node containing the current element |
| `begin()` is 0 | `begin()` is the front node |
| `end()` is $n$ (the index **after** the last element) | `end()` is the trailer node |

## sequences
when we combine the strengths of the array-based and the linked list-based implementations, we get sequences, which allow elements to be accessed by both index and position.

if we were to use a doubly-linked list implementation, then each node will contain the `Position` class, which contains a reference to the elements. using this: 
- position-based methods run in constant $O(1)$ time
- index-based methods will require searching, which makes it $O(n)$

by contrast, an array implementation will make use of the circular array. again, each cell of the array will contain a `Position` object, and the array will use the `f` and `l` integers to keep track of the first and last positions.

| operation | array sequence | list sequence |
| ---- | ---- | ---- |
| `size()`, `empty()` | $O(1)$ | $O(1)$ |
| `atIndex`, `indexOf`, `at` | $O(1)$ | $O(n)$ |
| `begin`, `end` | $O(1)$ | $O(1)$ |
| `set(position p, elem e)` | $O(1)$ | $O(1)$ |
| `set(int i, elem e)` | $O(1)$ | $O(n)$ |
| `insert(int i, elem e)`, `erase(int i)` | $O(n)$ | $O(n)$ |
| `push_back()`, `pop_back()` | $O(1)$ | $O(1)$ |
| `insert_front()`, `pop_front()` | $O(n)$ | $O(1)$ |
| `insert(position p, elem e)`, `erase(p)` | $O(n)$ | $O(1)$ |
