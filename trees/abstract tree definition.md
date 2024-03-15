a tree is a non-linear data structure which consists of: 
- a finite set of nodes (or vertices)
- a finite set of directed edges
when a tree is non-empty: 
- there must be 1 node that does not have incoming edge (indegree)
- every other node is reachable through some set through the root

all trees will have 1 singular **root** node, and each of its descendants are the roots of new subtrees. an abstract tree can be established using a hierarchy by using a class that: 
- stores a value
- stores children in a linked list.

each node of a tree will contain the following properties: 

```cpp
class TreeNode 
	private: 
		TreeNode * parent; 
		TreeNodeList * childrenContainer;
		E elem;
	public:
		E& operator*(); 
		Position parent(); 
		PositionList children(); 
		bool isRoot() const; 
		bool isExternal() const; 
}
```

then, a tree can be defined by the following operations: 

```cpp
class Tree {
	public: 
		class Position; 
		class PositionList;
		int size(); 
		bool empty(); 
		Position root(); 
		PositionList positions() const; 
}
```

## depth and height
the depth of a node in a tree are the number of levels from the root that node is. the runtime of the algorithm `depth(Tree T, Position p)` $O(d_{p})$

```cpp
int depth(const Tree& T, const Position& p) {
	if (p.isRoot()) {
		return 0; 
	} else {
		return 1 + depth(T, p.parent());
	}
}
```

by the same token, the height of a tree is the depth of the most external node. or, the greatest depth of any node of a tree. given this, we can define the method to get the height in two ways: 

method 1: using the `depth()` method on the external nodes of the tree, with TC $O(n + \Sigma_p(1 + d_p))$

```cpp
int height(const Tree& T) {
	int h = 0; 
	PositionList nodes = T.positions(); 
	for (Iterator q = nodes.begin(); q != nodes.end(); q++) {
		// get element of element at q
		if (q->isExternal()) {
			h = max(h, depth(T, *q));
		}
	}
	return h; 
}
```

or, by using recursion, with TC $O(\Sigma_p(1 + d_p))$: 

```cpp
int height2(const Tree& T, const Position& p) {  
	if (p.isExternal()) return 0;  
	int h = 0;  
	PositionList ch = p.children();
	for (Iterator q = ch.begin(); q != ch.end(); ++q) {  
		h = max(h, height2(T, *q);
	}
	return 1 + h;
```

