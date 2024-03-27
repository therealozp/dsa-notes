a binary tree must satisfy the following properties: 
- each node must have **at most** 2 children (or exactly 0 or 2 for proper binary trees)
- the children of the node are an ordered pair (any children of an internal node are annotated left and right)

there are many types of binary trees: 
- in a **full** (or proper) binary tree, nodes all have either 0 or 2 children.
- in a **perfect** binary tree, all branches have 2 children, and all leaves must be at the same depth
- in a **complete** binary tree, every level (except possibly the last) is completely filled, and pushed as far to the left as possible. 
- in a **balanced** binary tree, the left and right subtrees of all trees must only differ by at most 1. recursively speaking, the left and right subtrees of each node must also be balanced. 

## useful properties
let: 
- $h$ be the height of a tree T
- $n$ be the number of nodes
- $e$ be the number of external nodes
- $i$ be the number of internal nodes

then: 
1. $h + 1 \leq n \leq 2^{h + 1} - 1$ (the number of nodes is at least height + 1, and at most $2^{h+1} -1$)
2. $1 \leq e \leq 2^{h}$ - the number of external nodes are at least 1, and at most $2^{h}$
3. $h \leq i \leq 2^{h} - 1$ - the number of internal nodes are at least equal to height, and at most $2^{h}-1$
4. $log(n+1) - 1 \leq h \leq n -1$ - the height in a binary tree must be a minimum $log_{2}(n + 1) - 1$ (to account for the root) and at most $n-1$ (a linear case)
## implementations
a BinaryTree ADT can be implemented either as a linked structure or an array. it inherits all properties of the Tree ADT, but each node of a binary tree can only have two children `left` and `right`.

## a linked implementation
pros: 
- it uses space more efficiently
- provides additional flexibility
- each node has two links: 
then, a node of a BinaryTree will have the class prototype: 

```cpp
struct Node {
	Elem element; 
	Node* parent; 
	Node* left; 
	Node* right; 

	Node() : e(), parent(NULL), left(NULL), right(NULL) {};
}
```

and to get an iterator of the BinaryTree, we can use a `Position` class: 

```cpp
// position in the tree  
class Position {  
	private:  
		Node* v; // pointer to the node  
	public:  
		Position(Node* _v = NULL) : v(_v) {} // default constructor  
		Elem& operator*() { return v->element; } 
		
		// get left, right, parent child  
		Position left() const { return Position(v->left); }  
		Position right() const { return Position(v->right); }  
		Position parent() const { return Position(v->parent); }  
		
		// is root or external/leaf?  
		bool isRoot() const { return v->parent == NULL; }  
		bool isExternal() const {  
			return v->left == NULL && v->right == NULL;  
		}
	friend class LinkedBinaryTree; // give tree access  
};  
typedef std::list<Position> PositionList; // list of positions
```

then, a BinaryTree ADT can be implemented with: 

```cpp
class LinkedBinaryTree {  
	protected:
		class Node {...}
	public:
		class Position {...}
	public:  
		LinkedBinaryTree(); // constructor  
		int size() const; // number of nodes  
		bool empty() const; // is tree empty?
		  
		Position root() const; // get the root  
		void addRoot(); // add root to empty tree
		
		PositionList positions() const;
		void expandExternal(const Position& p);  
		
		// remove p and parent  
		Position removeAboveExternal(const Position& p);  
	protected: // local utilities  
		void preorder(Node* v, PositionList& pl) const;  
	private:  
		Node* _root; // pointer to the root  
		int n; // number of nodes  
};
```

the idea of the preorder function is to preorder traverse the binary tree, taking in a PositionList to store them along the way. 

the `expandExternal()` utility function is used whenever an insertion or a removal is used. 

```cpp
void expandExternal(const Position& p) {  
	Node* v = p.v; // p's node  
	
	v->left = new Node; // add a new left child  
	v->left->par = v; // v is its parent  
	
	v->right = new Node; // and a new right child  
	v->right->par = v; // v is its parent  
	
	n += 2; // two more nodes
}
```

by contrast, when we attempt to remove a parent of an external node, we can call the `removeAboveExternal()` function: 

```cpp
Position removeAboveExternal(const Position& p) {  
	// get p's node and parent  
	Node* w = p.v; 
	Node* parent = w->par;  
	Node* sib = (w == parent->left ? parent->right : parent->left);  
	// child of root?  
	if (parent == _root) {  
		_root = sib; // ...make sibling root  
		sib->par = NULL;  
	} else {  
		Node* gpar = parent->par; // w's grandparent  
		if (parent == gpar->left) {  
			gpar->left = sib; // replace parent by sib  
		} else {  
			gpar->right = sib;  
		}  
		sib->par = gpar;  
	}  
	delete w; delete parent; // delete removed nodes  
	n -= 2; // two fewer nodes  
	return Position(sib);  
}
```

### time complexity
the time complexity for the above operations in a binary tree are as follows: 

| operation                                         | time   |
| ------------------------------------------------- | ------ |
| `left`, `right`, `parent`, `isExternal`, `isRoot` | $O(1)$ |
| `size`, `empty`                                   | $O(1)$ |
| `root`                                            | $O(1)$ |
| `expandExternal`, `removeAboveExternal`           | $O(1)$ |
| `positions`                                       | $O(n)$ |

## array-based implementation
in contrast to the depth-first implementation with linked structures, we can store the nodes of a binary tree in an array. in this way: 
- the root node is usually stored at index 0. 
- for each node at index $i$:
	- the **left child's** position will be at $2i + 1$
	- the **right's** will be at $2i + 2$.

when using a 0-indexed `binaryTreeArray`, the parent of a node at index $i$ will be at $\lfloor \frac{i -1}{2} \rfloor$.

alternatively, we can also use an array and have the **root node stored at index 1**. this way, for each node at index $i$: 
- the **left child** will be at index $2i$
- the **right child** will be at index $2i + 1$
- its parent will be at $\lfloor \frac{i}{2} \rfloor$.

this method works especially well for perfect/complete binary trees, but not so well for sparser trees, as the space **wasted** can go up to $2^{h} - n$.

