#theory #linked-lists
## what's the difference between this and arrays?
- an array is a contiguous chunk of memory, while linked lists are a collection of unordered, linked **nodes**. 
- indexing in array takes $O(1)$, while linked lists take $O(n)$
- `insert()` and `delete()` in arrays take $O(n)$
- arrays have fixed size, linked lists do not.
## singly-linked lists
having a separate `Node` class helps it to be re-used later with subsequent different implementations.

```cpp
template <typename E>  
class Node { // singly linked list node  
	private:  
	E elem; // linked list element value  
	Node<E>* next; // next item in the list  
	friend class LinkedList<E>; 
	// provide SLinkedList access to the node’s private members
};
```

a singly-linked list consists of nodes that have a `next` pointer to the subsequent node in the linked list. the class definition of the linked list should have a reference to the `head` of the list. 

```cpp
template <typename E>  
class LList { 
	public:  
		LList(); // empty list constructor  
		~LList(); // destructor  
		bool empty() const; // is list empty?  
		const E& front() const; // return front element  
		void addFront(const E& e); // add to front of list  
		void removeFront(); // remove front item list  
	private:  
		SNode<E>* head; 
};
```
### alternative implementation
instead of having a reference to the `head` node, which leads to many subsequent problems when **the list is empty**, we could have a **sentinel** node representing the head. the value of this node is ignored, and it is not considered to be an actual element.

then, the implementation will always look something like: 
```cpp
class LList {
	private: 
		Node head(INT_MAX, nullptr);
	public: 
		// any class methods will go here
		usually, list ADTs will often have methods like: 
		- constructors / destructors
		- insert at head / tail / a specified position / in order
		- get item 
		- delete item
		- traverse
		- determine empty
}
```

### time complexity
| Operation | Front Node | $k^{th}$ node | $n^{th}$ node |
| ---- | ---- | ---- | ---- |
| Find | $\Theta(1)$ | $O(n)$ | $\Theta(1)$ |
| Insert before | $\Theta(1)$ | $O(n)$ | $O(n)$ |
| Insert after | $\Theta(1)$ | $\Theta(1)$ | $\Theta(1)$ |
| Replace | $\Theta(1)$ | $\Theta(1)$ | $\Theta(1)$ |
| Delete | $\Theta(1)$ | $O(n)$ | $O(n)$ |
| Next | $\Theta(1)$ | $\Theta(1)$ | $O(n)$ |
| Prev | n/a | $O(n)$ | $O(n)$ |
## doubly linked list
the only difference between a linked list and a doubly-linked list is that each doubly linked node has a reference to its **previous** node. DLLs are good for: 
- traversing in both directions
- reversing the linked list
- accessing a node from any point
however, this comes at the cost of: 
- **memory**: needs one extra slot for pointer to `prev`
- **performance**: always having to update both pointers on every operation

### implementation
as always, we can either take a reference directly to the `ListHead` and `ListTail` like singly-linked lists, or we can take a header-trailer approach which contains two **sentinels** at the front and the back. 

when deleting a node from the DLL, we simply re-route the two nodes before and after the deleted node to point to each other, made very simply by the `next` and `prev` pointers. a similar logical path can be taken when inserting a new node into the DLL. 

## circularly linked lists
in this ADT, there is no `ListHead`. the first node simply points to the last, and there is a loop. 

obviously, this means that traversal cannot be done normally, and instead must be done some other way. special care must be given to avoid **infinite loops**. **reversing**is also troublesome with CLLs. 

CLLs can be used for making an event thread that schedules programs, or represent turn-based games. we can also use CLLs to represent a **circular queue**. 
### initialization
we initialize the CLL with an empty list, with a `last` pointer. when adding a new element to the empty list, we set the `last->next` pointer to the new element and have its `next` point to itself. 

so, insertion into an empty list or the beginning of a list looks like: 

```cpp
n = new Node();
n->next = last->next;
last->next = n;
```

while inserting at the end of a list looks like: 

```cpp
n->next = last->next;
last->next = n;
last = n;
```

inserting in between nodes should occur normally as to how inserting in a singly-linked list would be. traversal, on the other hand, can be handled by starting from the `last->next` node, and stopping the iteration when it is reached a second time.

saira - wednesday last week
