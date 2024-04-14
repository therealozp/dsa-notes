## preorder traversal
a preorder traversal occurs when a node is visited before its children. in this manner, it is most similar to a graph depth-first search, where all lefts are visited linearly, before going on to the right subtrees and so on. use cases for preorder traversal are for printing structured documents, etc.

```cpp
void preorderPrint(const Tree& T, const Position& p) {  
	cout << *p; // print element  
	PositionList ch = p.children(); // list of children  
	for (Iterator q = ch.begin(); q != ch.end(); ++q) {  
		cout << " ";  
		preorderPrint(T, *q);  
	}
}
```

## postorder traversal
by contrast, postorder means that a node will be visited **after all of its descendants**. this is most useful when evaluating an arithmetic operation in postfix notation, or computing the space taken up by a directory. 

```cpp
void postorderPrint(const Tree& T, const Position& p) {  
	PositionList ch = p.children();
	for (Iterator q = ch.begin(); q != ch.end(); ++q) {  
		cout << " ";  
		postorderPrint(T, *q);  
	}
	cout << *p; // print element
}
```

## inorder traversal
unlike normal trees, binary trees also have one special traversal method, called inorder traversal. inorder traversal can be obtained by fully traversing the left subtrees, visit the node, then traversing the right subtree. 

```cpp
Algorithm inOrder(v) {  
	if v.isInternal() {  
		inOrder(v.left())  
	}  
	visit(v)  
	if v.isInternal() {  
		inOrder(v.right())
	}
}
```