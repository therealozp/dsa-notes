a map (also called associative array, map, symbol table, or dictionary) is a model of a searchable collection of **key-value** entries, with main operations `search()`, `insert()`, and `delete()` items. 

each key must be tied to a unique value, multiple entries with the same key are **not** allowed. 

## entry ADT
each entry ADT can be defined with the following parameters: 

```cpp
class Entry {
	private: 
		Key k; 
		Value v;

	public: 
		setKey(); 
		setValue(); 
}
```

## map ADT
similar to the map definition above, the map ADT should contain the following operations: 
- `find(k)`: returns an iterator to the entry with key $k$, else return an iterator to the end
- `put(k, v)`: updates the current entry with key $k$ with value $v$, creates a new entry if non-existent. returns an iterator to the newly-created entry.
- `erase(k)`: erases entry with key $k$ from the map if it exists

### a doubly-linked list implementation
we can use a DLL with each node being a reference to the entry `(k, v)`. an unsorted list works fine, but it is **very slow**. all operations will always be $O(n)$, since we always need to check if the key exists. 

this implementation is only effective when the `puts()` operation is most frequently used, like historical data records and suchlike.

```cpp
Algorithm find(k) {
	for each p in [S.begin(), S.end()) {
		if p->key() == k {
			return p
		}
	}
	return S->end() // there is no entry with key equal to k
}

Algorithm put(k,v) {
	for each p in [S.begin(), S.end()) {
		if p->key() == k {
			p->setValue(v)
			return p
		}
	}
	p = S.insertBack((k,v)) // there is no entry with key k
	n = n + 1
	 // increment number of entries
	return p
}

Algorithm erase(k) {
	for each p in [S.begin(), S.end()) {
		if p.key() == k {
			S.delete(p)
			n = n – 1 // decrement number of entries
		}
	}
}
```

### hashmaps
the hashmap consists a hash function h(x), which maps a given type to an integer in a given interval $[0, N - 1]$. 
e.g. $h(x) = x\mod N$ is a hash function for integer keys

then, the integer $h(x)$ is called the hash value of the key `x`. it will always consist of: 
- hash function $h(x)$
- array (or table) of size $N$

e.g. when designing a hash table for SSNs with size of 10000, we can use a hash function: $h(x) = x \mod 10000$, meaning it takes the last 4 digits of `x`.
### hash bucket (array)
a bucket for a hash table is **an array** A of size $N$, where each cell of A is a collection of **(key, value)** pairs, where $N$ defines the capacity of the array. 

if the hashed keys are well-distributed in the range of $[0, N-1]$, then this bucket array is all that is needed, where an entry with key $k$ is inserted into bucket `A[k]`.

if keys are unique in the specified range, then each bucket only holds at most one single entry. then, search, insertion, and removal all take place in $O(1)$ time. however: 
- the space used is $O(n)$, and if we don't have many entries, we end up wasting space.
- keys are required to be in the specified range, which is not often the case

$\rightarrow$ we normally use a bucket array with a *good* mapping with the keys to the integers. 

### hash functions
a hash function is specified as the composition of 2 functions: 
- hash code: $h_{1}= \text{keys} \rightarrow \text{integers}$
- compression function: $h_{2}= \text{integers}\rightarrow [0, N-1]$
then, the end function looks something like: $h_{2}(h_{1}(x))$. the goal of having the 2 functions are to disperse these keys in a seemingly random way. 
#### hash coding
**using memory addresses:**
- use memory addresses of the object keys as an integer
- good in general, except for numeric and string keys
**using integer cast:**
- bits of the key as an integer
- suitable for lengths of less than or equal to the number of bits in the integer type
**using component sum:**
- partition the bits of the key into components of fixed length (e.g., 16 or 32 bits)  
- we sum the components (ignoring overflows)  
- suitable for **fixed** lengths **greater than or equal** to the number of bits of the integer type
#### hash compression
- division: $h_{2}(y) = y \mod N$
	- the size $N$ is often chose to be a prime
	- to minimize collisions, its important to reduce the common factors between $N$ and the element count $y$.
- multiply, add, and divide (MAD): $h_{2}(y) = (ay + b) \mod N$
	- $a$ and $b$ must both be non-negative, such that $a \mod N \not = 0$
	- otherwise, every integer would map to the same value $b$

## collision handling
when multiple elements are mapped to a single cell, collision occurs. there are multiple ways to handle this, including: 
### separate chaining
let each cell in the table point to a **linked list**, and map everything there. this method is simple, but **requires extra memory outside the table.**

the **load factor**: if there are $n$ entries of our map into a bucket array of size $N$, we expect each bucket to be of size $\frac{n}{N}$.

### open addressing
the colliding item is placed on a different cell of the table. one form of open addressing is **linear probing**, by placing an item into $A[i + 1 \mod N]$.

collisions handled by placing the collided item (**often circularly**) into the next available table cell. the interval between each probe is **fixed**, and most often is 1. major disadvantage is that **all colliding items will lump together**, making future collisions probe even further.

another method is using **quadratic probing**, meaning that it uses a factor of $f(i) = i^{2}$ for determining where the item goes when it collides. then, the spot to place the item in will be: $A[i + f(i) \mod N]$. 

lastly, we can also use **double-hashing** to obtain another location. then, the space between the probes are **fixed**, but are computed by another hash function: $A[(i + f(j) \mod N)]$, where $j \in [1, N]$, and $f(j) = j * f'(\text{key}), \ i = \text{hash}(\text{key})$. 

## implementation

first, we define some type definitions that are useful: 

```cpp	
	protected:  
		typedef std::list<Entry> Bucket; // a bucket of entries  
		typedef std::vector<Bucket> BktArray; // a bucket array  
```

a `Bucket` is the list of entries that will eventually be chained together, and the `BucketArray` will be the outermost container.

```cpp
template <typename K, typename V, typename H>  
class HashMap {  
	public:  
		typedef Entry<const K,V> Entry; // a (key,value) pair  
		class Iterator; // a iterator/position 
		 
		HashMap(int capacity = 100); // constructor
		  
		Iterator find(const K& k); // find entry key k  
		Iterator put(const K& k, const V& v);// insert/replace 
		 
		void erase(const K& k); // remove entry key k  
		void erase(const Iterator& p); // erase entry at p  
		
		Iterator begin(); // iterator first entry  
		Iterator end(); // iterator end entry  
		
	private:  
		int n; // number of entries  
		H hash; // the hash comparator  
		BktArray B; // bucket array  
};
```

```cpp
// utility functions  
Iterator finder(const K& k);  
Iterator inserter(const Iterator& p, const Entry& e);  
void eraser(const Iterator& p);  

typedef typename BktArray::iterator BItor;  
typedef typename Bucket::iterator EItor;  

static void nextEntry(Iterator& p) { ++p.ent; }  
static bool endOfBkt(const Iterator& p) {  
	return p.ent == p.bkt->end(); 
}
```

the `Iterator` class looks something like as follows: 

```cpp
// an iterator (& position)  
class Iterator {  
	private:  
		EItor ent; // which entry  
		BItor bkt; // which bucket  
		const BktArray* ba; // which bucket array
		  
	public:  
		Iterator(const BktArray& a, const BItor& b,  
		const EItor& q = EItor()) : ent(q), bkt(b), ba(&a) { }
		  
		Entry& operator*() const; // get entry  
		bool operator==(const Iterator& p) const;   
		Iterator& operator++(); // advance to next entry 
		 
		friend class HashMap; // give HashMap access
```

to compare if the two iterators in the hashmap are equal, we first check if they are in the same bucket, if they're both at the end of the hashmap. if both pass, we can simply compare the two entries. 

```cpp
// are iterators equal?  
template <typename K, typename V, typename H>  
bool HashMap<K,V,H>::Iterator::operator==(const Iterator& p)  
const {  
	// ba (Bucket Array) or bkt (Bucket) differ?  
	if (ba != p.ba || bkt != p.bkt) return false;  
	// both at the end?  
	else if (bkt == ba->end()) return true;  
	// else use entry to decide  
	else return (ent == p.ent);
```

to return a location to the next entry, we first start by incrementing the `Entry` iterator. if after incrementing and it happens to go out of bounds, we need to traverse to the next non-empty bucket. if there are no non-empty buckets, then we return `end()`, else we return a reference to the element.

```cpp
// advance to next entry  
template <typename K, typename V, typename H>  
typename HashMap<K,V,H>::Iterator&  
HashMap<K,V,H>::Iterator::operator++() {  
	// next entry in bucket  
	++ent;  
	// at end of bucket?  
	if (endOfBkt(*this)) {  
		// go to next bucket  
		++bkt;  
		// find nonempty bucket  
		while (bkt != ba->end() && bkt->empty()) {
			++bkt; 
		}  
		// end of bucket array?  
		if (bkt == ba->end()) return *this;  
		// first nonempty entry  
		ent = bkt->begin();  
	}  
	return *this; // return self  
}
```

```cpp
template <typename K, typename V, typename H>  
HashMap<K,V,H>::HashMap(int capacity) : n(0), B(capacity) { }  
// number of entries  
template <typename K, typename V, typename H>  
int HashMap<K,V,H>::size() const {  
	return n;  
}  
// is the map empty?  
template <typename K, typename V, typename H>  
bool HashMap<K,V,H>::empty() const {  
	return size() == 0;
}
```

```cpp
// iterator to end  
template <typename K, typename V, typename H>  
typename HashMap<K,V,H>::Iterator HashMap<K,V,H>::end() {  
	return Iterator(B, B.end());  
}  
// iterator to front  
template <typename K, typename V, typename H>  
typename HashMap<K,V,H>::Iterator HashMap<K,V,H>::begin() {  
	if (empty()) return end();  
	
	BItor bkt = B.begin();  
	while (bkt->empty()) { ++bkt; }  
	return Iterator(B, bkt, bkt->begin());
}
```

### searching for element - `find()`
naturally, we first need to compute the hash of the key, and then computing its modulo to get the $i^{th}$ bucket. we linear probe until the end of the current bucket using the `nextEntry()` utility function, and return it.

```cpp
template <typename K, typename V, typename H>  
typename HashMap<K,V,H>::Iterator HashMap<K,V,H>::finder(const  
K& k) {  
	int i = hash(k) % B.size(); // get hash index i  
	BItor bkt = B.begin() + i; // the ith bucket  
	Iterator p(B, bkt, bkt->begin()); // start of ith bucket  
	// search for k  
	while (!endOfBkt(p) && (*p).key() != k) { 
		nextEntry(p); 
	}  
	return p; // return final position  
}  
// find key  
template <typename K, typename V, typename H>  
typename HashMap<K,V,H>::Iterator HashMap<K,V,H>::find(const K&  
k) {  
	Iterator p = finder(k); // look for k  
	if (endOfBkt(p)) { // didn't find it?  
		return end(); // return end iterator  
	} else {  
		return p; // return its position  
	}  
}
```

### inserting elements - `put()`

```cpp
// insert utility  
template <typename K, typename V, typename H>  
typename HashMap<K,V,H>::Iterator HashMap<K,V,H>::inserter(const  
Iterator& p, const Entry& e) {  
	EItor ins = p.bkt->insert(p.ent, e); // insert before p  
	n++; // one more entry  
	return Iterator(B, p.bkt, ins); // return this position  
}  
// insert/replace (v,k)  
template <typename K, typename V, typename H>  
typename HashMap<K,V,H>::Iterator HashMap<K,V,H>::put(const K&  
k, const V& v) {  
	Iterator p = finder(k); // search for k  
	if (endOfBkt(p)) {
		// didn't find k, so insert at end  
		return inserter(p, Entry(k, v)); 
	} else { // found it?  
		p.ent->setValue(v); // replace value with v  
		return p; // return this position  
	}  
}
```

### deleting element - `erase()`

```cpp
// remove utility  
template <typename K, typename V, typename H>  
void HashMap<K,V,H>::eraser(const Iterator& p) {  
	p.bkt->erase(p.ent); // remove entry from bucket  
	n--; // one fewer entry  
}  
// remove entry at p  
template <typename K, typename V, typename H>  
void HashMap<K,V,H>::erase(const Iterator& p) {  
	eraser(p);  
}  
// remove entry with key k  
template <typename K, typename V, typename H>  
	Iterator p = finder(k); // find k  
	if (endOfBkt(p)) { // not found?  
		throw NonexistentElement("Erase of nonexistent");  
	}  
	eraser(p)
}
```