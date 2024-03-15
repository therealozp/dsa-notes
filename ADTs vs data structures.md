an abstract data type (or an ADT for short) is a high-level view of a concept independent of its implementation, defined by its **values** and its **functions.** by contrast, a data structure is a concrete implementation, complete with its own public/private access modifiers, internal values, etc.

as an example, a container is an ADT, but a stack, a queue, or a linked list is a data structure. 

an ADT is viewed from the perspective of the **user**, meaning it should have x, y, z function, while the data structure is taken from the perspective of the **implementer.**

the most vital part of an ADT is its **encapsulation**, meaning that there should be no pointer that can access the internal data of the data structure from outside. an insertion into the list should be taken as a *copy* of the original, and retrieval from the ADT should return a *copy* of what is in that list, instead of the actual reference to the element.

ADT design should follow 5 main principles: 
- S: **single responsibility**. each class should only solve 1 problem, and other classes should be tasked with theirs.
- O: **open-closed design**. it should be open for extension, but closed for modifications.
- L: **Liskov's substitution principle**. this essentially means that any replacement of a type with its subtype (e.g. integers and natural numbers) should not break the implementation
- I: **interface segregation**. it's better to have many specialized clients than one general one.
- D: **dependency inversion**. base classes should not depend on derived classes, but both should depend on the ADT. abstractions should not depend on details, but the details should depend on the abstraction.

polymorphism essentially means many functions having the same name can perform different operations. in C++, functions can be overloaded to take a different number of arguments, etc, while class methods can be bound dynamically with the `virtual` keyword. 