#oop #deque #stack 
## idea
we must first understand that a deque implementation needs 4 (or 6) core functions:

```cpp
push_back(); 
push_front(); 
pop_back(); 
pop_front(); 
// front(); to peek front
// back(); to peek back
```

so, by using 2 stacks, we can use the first stack for read operations, and when any write operation is made, we can update the other stacks accordingly. 

```cpp
class deque_stacks: 
	private: 
		stack<T> aux_stack, main_stack; 
	public: 
		void push_back(T elem) {
			main_stack.push(elem); 
		}

		void push_front(T elem) {
			// move all elements from stack 2 to stack 1
			while (!main_stack.empty()) {
				aux_stack.push(main_stack.pop());
			}
			// push on top of element
			main_stack.push(elem); 
			// move all elements back from aux_stack to main_stack
			while (!aux_stack.empty()) {
				main_stack.push(aux_stack.pop()); 
			}
		}

		T pop_back()  {
			return main_stack.pop();
		}

		T pop_front() {
			while (!main_stack.empty()) {
				aux_stack.push(main_stack.pop());
			}
			T r_val = aux_stack.pop();
			while (!aux_stack.empty()) {
				main_stack.push(aux_stack.pop());
			}
			return r_val; 
		}
```

any operation related to `front()` will cost $O(n)$ time, while those from the `back()` will take $O(1)$.