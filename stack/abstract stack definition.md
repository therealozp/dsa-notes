#theory #stack
A last-in-first-out data structure, much like a stack of plates or organized laundry. it has two main operations, both taking $O(1)$ time: 
- `push()`: appends an element to the end of the stack. 
- `pop()`: removes the topmost element in the stack. 
## implementations
since all operations in the front (`addToFront`, `deleteHead`) of a **linked list** takes $\Theta(1)$ time, we might consider implementing a linked-list stack. 

alternatively, all operations at the back of an array also takes average $O(1)$ time, so an array can also be used to implement a stack. 

## stack overflow / underflow
a call stack is normally used to store information about subroutines. when there are too many routines going on in the stack, a stack overflow occurs. 

on the other hand, a stack underflow occurs when we try to perform a `pop()` operation on an empty stack. 