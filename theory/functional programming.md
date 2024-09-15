## declarative vs imperative
imperative programming involves some change of state $\rightarrow$ object-oriented programming.

declarative programming expresses the logic of a computation without describing its control flow.

```cpp
// Imperative Doubling  
function double (arr) {  
	let results = []  
	for (let i = 0; i < arr.length; i++){  
		results.push(arr[i] * 2)  
	}  
	return results  
}

// Declarative Doubling  
function double (arr) {  
	return arr.map((item) => item * 2)  
}
```

we assume `map()` has some imperative code that applies the lambda function to the members of the array. many declarative approaches have some sort of underlying **imperative abstraction**.  

the four principles of functional programming: 
- [[#predictability]]
- [[#safety]]
- [[#referential transparent]]
- [[#modularity]]
## predictability
- the result of a function depends on the arguments
- no side effects (I/O, mutation of state)
- aims to be declarative, not imperative
- gives us some level of predictability how operations will perform
## safety
- **NO MUTABLE STATES**
- the result of a pure function can be removed without affecting other functions
- if the function is called without causing any side-effects, the result will be constant. calling the function again with the same arguments will produce the same results.
- if there is no data dependency between two functions, their order can be **changed** without interfering with one another (built-in thread safety)
## side-effects
a side effect is a **change of state** or observable interaction with the outside world that occurs during the calculation of a result, e.g. 
- changing the file system  
- inserting a record into a database  
- making an `http` call  
- mutations  
- printing to the screen / logging  
- obtaining user input  
- accessing system state
## pure vs impure functions
functions that interact with global functions or cause side effects are considered impure, everything else is pure.

```cpp
const xs = [1,2,3,4,5];  
// pure  
xs.slice(0,3); // [1,2,3]  
xs.slice(0,3); // [1,2,3]  
xs.slice(0,3); // [1,2,3]

// impure  
xs.splice(0,3); // [1,2,3]  
xs.splice(0,3); // [4,5]  
xs.splice(0,3); // []  

// impure  
let minimum = 21;  
const checkAge = age => age >= minimum;  

// pure  
const checkAge = (age) => {  
	const minimum = 21; 
	return age >= 21;
}
```

pure functions give a number of advantages. they are: 
- **cacheable**: they can always be cached by input. this is typically done using a technique called *memoization*, which is storing the inputs and returning the cached output when the same inputs occur.
- **portable**: they are self-contained
- **testable**: 
- **parallel code**: we can run pure functions in parallel and it does not need access to shared memory, so it is guaranteed against race conditions.

## referential transparent
the value of a variable in a functional program will never change once defined. therefore, this eliminates any chance of side effects because any variable can be replaced with its actual value at any point in the execution cycle.

## modularity
functional programming uses the idea of functions as building blocks, so we can take these pieces and put them together.

currying and partial application are common ways to build functions into more complicated designs.

## first class functions
A function is considered first class if it supports the following operations:
- assignment to a variable
- being passed as arguments for higher order functions
- being returned from a function
## higher order functions
a function that does one of the following: 
- takes one or more function as a argument
- returns a function as a result

requires first-class functions.
for example, the differential operator is a higher order function, because it takes in a function, and returns another function.
### common higher-order functions
### `map()`
performs the exact same operation on each of the elements in the array and return the same amount of items in the array. 

the function performing these operations is called the **callback function**, which **requires a return value.** the callback function can take in three values:
- the current item
- the index of the current item
- the entire array
**note**: the callback function must return a value, otherwise it will be undefined.
### `filter()`
filters out the elements that matches the criteria for the function specified. the callback function must return a boolean.

```js
// Imperative  
let small_animals = [];  
for (let i = 0; i < animals.length; i ++) {  
	if (animals[i].size === "small") {  
		small_animals.push(animals[i])  
	}  
}  

// Declarative  
const small_animals = animals.filter((animal) => {  
	return animal.size === "small"  
})
```

### `reduce()`
uses the elements in the array to create a completely new, **single** value. the callback function is specified differently: 
- the first parameter is the **current value of the end value**. we can set this at initialization, in this case, it is set to 0.
- the second parameter is the current element
- the third parameter is the index
- the last is the full array.

the callback for `reduce()` requires a return value of the **end value**, so that it can be compounded further.

```js
// Imperative  
let total_weight = 0;  
for (let i = 0; i < animals.length; i++) {  
	total_weigth += animals[i].weight  
}

// Declarative  
let total_weight = animals.reduce((weight, animal) => {  
	return weight += animal.weight  
}, 0)
```

## closure
formally speaking, a closure is a technique for implementing lexically-scoped name-binding in a language with first-class functions, and is a persistent local variable scope.

consider the below function, which takes in `x` and returns a function which will take in a value `y`: 

```js
const createAdder = (x) => {
	return (y) => x + y;
}

const add3 = createAdder(3);
```

## currying
currying is basically calling a function with fewer arguments than it expects. it returns a function that will take in the remaining arguments. with currying:
- little pieces can be configured and reused with ease  
- functions are used throughout  
- encourages the creation of functions; rather than of methods.  
- large expressive power.

this makes extensive use of closures, as it remembers the first argument passed in. consider the quicksort algorithm implemented in JavaScript: 

```js
const lessEqualThan = y => x => (x <= y);  
const greaterThan = y => x => (x > y);  
const firstElement = arr => arr[0];  
const arrayWithoutFirst = arr => arr.slice(1);  
const filterByFunction = func => arr => arr.filter(func);

const mergeToArray = item => nextItem => (  
	nextItem === undefined ? item  
	: mergeToArray([].concat(item, nextItem))  
);

const filterByLTE = val => arr => (  
	filterByFunction(lessEqualThan(val))(arr)  
);

const filterByGT = val => arr => (  
	filterByFunction(greaterThan(val))(arr)  
);

const quicksort = (arr) => {  
	if (arr.length === 0) return [];  
	const pivot = firstElement(arr);  
	const subarray = arrayWithoutFirst(arr);
	  
	const left = quicksort(filterByLTE(pivot)(subarray));  
	const right = quicksort(filterByGT(pivot)(subarray));
	  
	return mergeToArray(left)(pivot)(right)();  
};
```

## compose and pipe
function composition is especially effective at combining smaller pieces into a more complex function, passing one's result to the next. consider the following function:

```js
const getName = person => person.name;  
const uppercase = string => string.toUpperCase();  
const get6Characters = string => string.substring(0, 6);

get6Characters(uppercase(getName({ name: 'Buckethead' })));
```

the same functionality achieved with `pipe()`:

```js
pipe(  
getName,  
uppercase,  
get6Characters,  
)({ name: 'Buckethead' })
```

and the same functionality achieved with `compose()`:

```js
const newName = compose(get6Characters, uppercase, getName);
```

```js
const toUpperCase = x => x.toUpperCase();  
const exclaim = x => `${x}!`;  
const shout = compose(exclaim, toUpperCase); // returns SHOUT
```

## pointfree style
pointfree style means never having to say what data you work with, meaning functions will never mention the data on which they operate. the building blocks of this style is first-class functions, currying, and composition.

pointfree code can helps us remove needless names and keep us concise and  
generic.

```js
// not pointfree because we mention the data: word  
const snakeCase = word => (  
	word.toLowerCase().replace(/\s+/ig, '_')  
);  
// pointfree  
const snakeCase = compose(replace(/\s+/ig, '_'), toLowerCase);

// not pointfree because we mention the data: name  
const initials = name => (  
	name.split(' ').map(compose(toUpperCase, head)).join('. ');  
)  
// pointfree  
const initials = compose(  
	join('. '),  
	map(compose(toUpperCase, head)),  
	split(' ')  
);
```