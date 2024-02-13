#theory #arrays
a collection of items in a linear order is called a **list**, or a **sequence.** and a sequence that is supported by accessing elements by means of **indexing** is called a **vector**, aka. dynamic arrays, vector lists, etc.

in its most basic form, a vector should support these operations: 

```cpp
at(int i); // return the element V with index i
set(int i, struct elem e); // replaces the element at index i with e
insert(int i, struct elem e); // inserts a new element e with index i (throws error if out of range)
erase(int i); // removes element at index i
```

for example: 

| operation | output | v |
| ---- | ---- | ---- |
| insert(0, 7) | - | [7] |
| insert(0, 4) | - | [4, 7] |
| at(1) | 7 | [4, 7] |
| insert(2, 2) | - | [4, 7, 2] |
| at(3) | *error* |  |
| erase(1) | -  | [4, 2] |
here, we note that the `insert()` operation will push all elements with indices $\geq$ i forward one, while the `erase()` operation pushes everything $>$ i backwards.

## implementation
there are a few things to be careful about when implementing the vector ADT. 
- accessing the `at()` operator should throw an error when out of bounds
- should have a `reserve()` operation to double size every time insertion goes over capacity.

the class definition should look something of this sort: 

```cpp
class ArrayVector {  
	public:  
		ArrayVector(); 
		int size() const; 
		bool empty() const; 
		Elem& operator[](int i); // element at index with overloading
		Elem& at(int i) throw(IndexOutOfBounds); 
		void erase(int i); // remove element at index  
		void insert(int i, const Elem& e); // insert element at index  
		void reserve(int N); // reserve at least N spots  

	private:  
		int capacity; 
		int n; // number of elements in vector  
		Elem* A; // array storing the elements  
	};
```

```cpp
void ArrayVector::reserve(int N) {  
	if (capacity >= N) return; // already big enough  
	Elem* B = new Elem[N]; // allocate bigger array  
	// copy contents to new array  
	for (int j = 0; j < n; j++) { 
		B[j] = A[j]; 
	}  
	// discard old array  
	if (A != NULL) {
		delete [] A;
	}  
	A = B; // make B the new array  
	capacity = N; // set new capacity  
}
```

as stated above, insertion should first check for over-capacity, and then shift every element up and insert into the empty slot.

```cpp
void ArrayVector::insert(int i, const Elem& e) {  
	// Do we need more space? If so, double array size  
	if (n >= capacity) { 
		reserve(max(1, 2 * capacity)); 
	}  
	// shift elements up  
	for (int j = n - 1; j >= i; j--) {
		A[j + 1] = A[j]; 
	}  
	A[i] = e; // put in empty slot  
	n++; 
}
```

and the erase operator should do the inverse of that: 

```cpp
void ArrayVector::erase(int i) {  
	// shift elements down  
	for (int j = i + 1; j < n; j++) {  
		A[j - 1] = A[j];  
	}  
	n--; // one fewer element  
}
```

for operations on lists or linked lists, we implement an [iterator](lists,%20iterators,%20and%20sequences.md) to loop through the data structure.