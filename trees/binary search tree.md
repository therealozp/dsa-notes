binary search function can perform searching on a **sorted** array-based sequence. at each step, the number of candidates for searching is halved, meaning its time complexity is $O(\log(n))$

below is a recursive definition of the binary search function: 

```cpp
int binarySearch(int array[], int left, int right, int target){  
	if (right >= left) {  
		int middle = left + (right - left) / 2;  
		// target is the middle element  
		if (array[middle] == target) return middle; 
		
		if (array[middle] > target) {  
			return binarySearch(array, left, middle - 1, target);  
		}  else {
			return binarySearch(array, middle + 1, right, target);  
		}
	}  
	// target is not in the array  
	return -1;
}
```

a non-recursive definition: 

```cpp
int binarySearch(int array[], int target) {
	int left = 0;
	int right = array.size() - 1; 
	int middle = 0;
	while (left < right) {
		middle = left + (right - left) / 2; 
		if (array[middle] == target) {
			return middle;
		} 
		
		if (array[middle] > target) {
			right = middle - 1; 
		} else {
			left = middle + 1;
		}
	}
	
	return -1
}
```

then, a BST is a visual representation of the above method, by using keys in a tree that satisfy the following property:

> for every node `u`, `v`, and `w`, if `u` is in the left subtree and `w` is in the right subtree of `v`, then `key(u)` $\leq$ `key(v)` $\leq$ `key(w)`

then, an inorder traversal of binary trees will visit the keys in **increasing** order (if all elements are distinct) or non-decreasing order (if there are duplicates).

## implementation
depending on the implementation, there are many different ways to represent a BST. however, we will consider that the last layer of nodes (external) nodes all contain empty values, as sentinels.

```cpp
template <typename E> class SearchTree {  
	public:  
		typedef typename E::Key K; // a key (from Entry in Maps)  
		typedef typename E::Value V; // a value (from Entry in Maps)  
		class Iterator; // an iterator/position
	
	public: 
		SearchTree(); // constructor
		  
		int size() const; // number of entries  
		bool empty() const; // is the tree empty?
		  
		Iterator find(const K& k); // find entry with key k  
		Iterator insert(const K& k, const V& x); // insert (k,x)
		  
		// remove key k entry  
		void erase(const K& k) throw(NonexistentElement);  
		// remove entry at p  
		void erase(const Iterator& p);
		  
		Iterator begin(); // iterator to first entry  
		Iterator end(); // iterator to end entry
		
	
	// local utilities
	protected: 
		// a linked-based binary tree
		typedef BinaryTree<E> BinaryTree;  
		// position in the tree  
		typedef typename BinaryTree::Position TPos;  
		TPos root() const; // get virtual root (super root)
		  
		TPos finder(const K& k, const TPos& v); // find utility  
		TPos inserter(const K& k, const V& x); // insert utility
		  
		TPos eraser(TPos& v); // erase utility  
		TPos restructure(const TPos& v) throw(BoundaryViolation);

	private: 
		BinaryTree T; 
		int n; 
```

the `Iterator` class is more complicated, as when traversing the iterator, we must follow a set of rules.

```cpp
Class Iterator {  
	private:  
		TPos v; // which entry  
	public:  
		Iterator(const TPos& vv) : v(vv) { } // constructor
		  
		// get entry (read only)  
		const E& operator*() const { return *v; }  
		// get entry (read/write)  
		E& operator*() { return *v; }
		  
		// are iterators equal?  
		bool operator==(const Iterator& p) const { return v == p.v;}  
		Iterator& operator++(); // next node in inorder   
	friend class SearchTree; // give search tree access  
};
```

the `++` operator means the next node in the inorder traversal starting from that node, so we must account for that accordingly: 

```cpp
typename SearchTree<E>::Iterator& SearchTree<E>::Iterator::operator++() { 
	TPos w = v.right();  
	if (w.isInternal()) { // have right subtree?  
		do { // move down left chain  
			v = w;  
			w = w.left();  
		} while (w.isInternal());  
	} else {  
		w = v.parent(); // get parent  
		while (v == w.right()) { // move up right chain  
			v = w;  
			w = w.parent();  
		}  
		// and first link to left  
		v = w;  
	}  
	return *this;  
}
```

however, there is an issue with this implementation: if we are currently at the rightmost node of the tree, then the `++` operator will keep on looping until the root of the tree. since the root has no parent, the node will turn `NULL`. 

to avert this problem, we will initialize the tree with a **super root**, and the actual root of the tree will be its left child. then, the `end()` iterator will point to that super root, since the rightmost node has no successor.

```cpp
// constructor (create the super root)  
template <typename E> SearchTree<E>::SearchTree() : T(), n(0) {  
	T.addRoot();  
	T.expandExternal(T.root());  
}

// get virtual root (left child of super root)  
template <typename E>  
typename SearchTree<E>::TPos SearchTree<E>::root() const {  
	return T.root().left();  
}  

void LinkedBinaryTree::addRoot() { _root = new Node; n = 1; }
```

```cpp
template <typename E> // iterator to first entry  
typename SearchTree<E>::Iterator SearchTree<E>::begin() {  
	TPos v = root(); // start at virtual root  
	while (v.isInternal()) v = v.left(); // find leftmost node  
	return Iterator(v.parent());  
}  
// iterator to end entry  
template <typename E>  
typename SearchTree<E>::Iterator SearchTree<E>::end() {  
	return Iterator(T.root()); // return the super root  
}
```

## search
searching for a key in a BST is similar to how the binary search algorithm works. starting at the root, we go a downward path along the branches, if the current node is greater than what we're searching for, we go right. else, we go left.

if we eventually find the node, then we can return an `Iterator` to that position. else if we reach a leaf node, then we can conclude that the element was not found.

```cpp
// helper function
SearchTree<E>::TPos finder(const K& k, const TPos& v) {  
	if (v.isExternal())  
		return v; // key not found  
	if (k < v->key())  
		return finder(k, v.left()); // search left subtree  
	else if (v->key() < k)  
		return finder(k, v.right()); // search right subtree  
	else return v; // found it here  
}

// public function  
SearchTree<E>::Iterator SearchTree<E>::find(const K& k) {  
	TPos v = finder(k, root()); // search from virtual root  
	if (v.isInternal())  
		return Iterator(v); // found it  
	else  
		return end(); // didn't find it  
}
```

## insertion
to perform the `insert()` operation, we first search for the key $k$ we are looking to insert. from then, we reach 2 cases: 
1. the element **is not** in the tree. then, let `w` be the leaf node reached by the search function. then, we simply put $k$ there, and call `expandExternal()` on it to make 2 more leaf nodes.
2. the element **is** in the tree. then, we insert it into the **right** subtree of the reached node.

```cpp
// helper function
SearchTree<E>::TPos inserter(const Key& k, const Value& x) {  
	TPos v = finder(k, root()); // search from virtual root
		  
	while (v.isInternal()) // key already exists?  
		v = finder(k, v.right()); // look further
		  
	T.expandExternal(v); // add new internal node  
	v->setKey(k); // set entry  
	v->setValue(x);  
	n++;  
	return v;   
}  
// insert (k,x)  
SearchTree<E>::Iterator
SearchTree<E>::insert(const Key& k, const Value& x) {  
	TPos v = inserter(k, x);  
	return Iterator(v);
}
```

## erase
to perform the `erase()` operation, we first search for the key $k$. assuming $k$ is in the tree. and `v` be the node with said key. then, we run into one of 2 cases: 
1. if `v` has a leaf child `w`, we remove `v` and `w` from the tree with the operation `removeExternal(w)`, which knocks `w` and its parent.
2. if `v` and its children are all internal, we then follow a process: 
	- find the internal node `w` that follows `v` in an inorder traversal
	- copy the key of `w` into `v`
	- then, return the node `w` and its left child `z` (which, at this point, must be a leaf) by calling `removeExternal(w)`

```cpp
// remove key k entry  
template <typename E>  
void SearchTree<E>::erase(const K& k) throw(NonexistentElement) {  
	TPos v = finder(k, root()); // search from virtual root  
	if (v.isExternal()) // not found?  
		throw NonexistentElement("Erase of nonexistent");  
	eraser(v); // remove it  
}

// erase entry at Position p  
template <typename E>  
void SearchTree<E>::erase(const Iterator& p) {  
	eraser(p.v);  
}

// helper function
template <typename E>  
typename SearchTree<E>::TPos SearchTree<E>::eraser(TPos& v) {  
	TPos w;  
	if (v.left().isExternal()) {  
		w = v.left(); // remove from left  
	} else if (v.right().isExternal()) {  
		w = v.right(); // remove from right  
	} else { // both internal?  
		w = v.right(); // go to right subtre  
		do {  
			w = w.left();  
		} while (w.isInternal()); // get leftmost node  
		TPos u = w.parent();  
		// copy w's parent to v  
		v->setKey(u->key());  
		v->setValue(u->value());  
	}  
	// one less entry and remove w and parent  
	n--;  
	return T.removeAboveExternal(w)
}
```
