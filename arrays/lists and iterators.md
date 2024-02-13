#theory #arrays
other than using **indexing** to reach elements in a linear structure, we can also implement a **list ADT** and use a **position** element that abstracts the notion of "place" in a list. 

in essence, the list ADT uses the linked list under the hood alongside another ADT that abstracts "place". however, this approach means that indexing will take **linear time** instead of constant as in arrays.

## position ADT
the position ADT basically gives a unified view of how we view "place" in a data structure, such as a cell in an array or a node in a linked list. 

the only method that this ADT should need is: 

```cpp
object p.element(); // returns the reference to the element stored at this position
// or, we can also overload the * operator: 
object *p; 
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
we have discussed the [deque](queues/abstract%20queue%20definition.md), and using that as a base, we add on the iterator datatype to loop through the structure. intrinsically, it should have the following members: 

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
		T& operator*(); // reference to the element  
		bool operator==(const Iterator& p) const; // compare positions  
		bool operator!=(const Iterator& p) const;  
		Iterator& operator++(); // move to next position  
		Iterator& operator--(); // move to previous position  
		friend class NodeList; // give NodeList access  
		
	private:  
		Node* v; // pointer to the node  
		Iterator(Node* u); // private constractor create from node
```

within this definition: 
- the first line overloads the `*` operator, returning the **address** of the element. 
- the second and third lines implement a comparison check, taking in another address of the iterator to compare.
- the fourth and fifth line defines operators to move forwards and backwards, of course returning an address operator (because the `++` operation essentially means `p = p + 1`) 
- the last line gives only the `NodeList` class access to the element.

