> for every input, the algorithm must terminate with a solution

## proof of correctness
1. start with *arbitrary* input
2. describe what algorithm does
	- `if` statements; proof by cases
	- `for/do/while`: requires **loop invariants**
	- recursion: induction or strong induction
	- other function calls: assume achieves subgoals/subtasks
3. prove that the output meets problem criteria (and that it terminates)

alternatively, a **proof of contradiction** may also be used:
1. assume the claim is false
2. show something impossible/logically inconsistent must be true
3. used in **optimization** problems: (minmax, best/worst)

## weak induction
normally used for statements of form $P(i), \forall\  i \geq b$. 
1. define the **base case**: prove that the claim is true at b
2. **induction hypothesis** (weak): suppose that the claim is true for some $k\geq b$.
3. **induction goal**: prove claim for $k + 1$ 

### variants:
- strong induction changes the induction hypothesis: "suppose that the claim is true **for all** $n$ from $b$ up to $k$."

- multiple base cases:
	- needed for strong induction proofs, etc.
### example
if $p(n)$ is the proposition that the sum of the first $n$ positive integers is $\frac{n(n + 1)}{2}$, prove $p(n)$ for $n \in Z^{*}$. 

**base case**: show $p(1)$ is true.
**inductive step**: for an arbitrary $k$, assume that $p(k)$ is true and prove that $p(k + 1)$ is correct.
**induction hypothesis**: 
$$
\begin{equation}
\begin{split}
1 + 2 + 3 + 4 + \cdots + k =&& \frac{k(k+1)}{2} \\
\Rightarrow 1 + 2 + 3 + 4 + \cdots + k + 1 =&&  \frac{k(k+1)}{2} + \frac{2k + 2}{2}
\end{split}
\end{equation}
$$

## strong induction

> prove that a pile of $n$ stones will always take $n-1$ moves to split into $n$ piles of $1$ stone.

- suppose that there is a pile of k + 1 stones. we proceed to split this into two piles of size $a$ and $b$. then, $a + b = k + 1$
- since $a < k$ and $b < k$, this would take $a - 1$ and $b - 1$ moves respectively to fully split.
- then, the total number of moves made will be:
$$1 \text{ (for the original split)} + (a - 1) + (b - 1) = a + b - 1 = k + 1 - 1 = k$$
### example
**base case**: for $n = 1$, array only contains one element, which is trivially sorted.
**induction hyp.**: assume that MergeSort already correctly sorts all elements up to length $k$.
**inductive step**: proves that it also works for $k + 1$:
- divide: it divides it into two halves, both with lengths $< k$
- recurse: by the inductive hyp, MergeSort will correctly sort these two halves, because they are both $< k$.
- merge: this operation linearly goes through two halves, thus the order is preserved.
$\rightarrow$ the array is sorted correctly.
## weak vs strong induction
**weak induction**: used when each step only depends on the immediately preceding steps (assume for any arbitrary $k$, prove $k + 1$)
**strong induction**: used when each step may depend on *any or all* of the previous steps (assume for **all items up to $k$**, prove $k + 1$)
## loop invariant
a loop invariant is **a property or a condition that holds true and before and after each iteration** of a loop. normally, it would be something like "this is what the loop has done so far."

it has the following properties: 
- **initialization**: the LI is true before the first iteration of the loop. 
- **maintenance**: if the LI is true at the start of one iter, then it should also be true at the start of the next iter.
- **termination**: when the loop terminates, LI gives a useful property to prove correctness.

in other words, it is a **statement** that must be true:
- before the first iter
- at the end of any iter, if it is true at the start
- at the very end of the loop.

to prove a loop invariant:
- prove that it is true for the $k^{th}$ operation.
- prove that it is also true for the next operation.

consider the below function:

```python
def exp(x, n):
	i = 0
	ans = 1
	while i < n:
		ans *= x
		i += 1
	return ans
```

in this case, the LI is:
- $i < n$? wrong, because at the end, $i = n$
- $i \in [0, n]$: correct, but irrelevant.
- computes $x^{n}$: no.
- `ans = x^i`: yes.

```python
def selection_sort(arr):
	for i in range(n):
		min_index = i
		for j in range(i + 1, n):
			if arr[j] < arr[min_index]:
				min_index = j
		swap(arr[i], arr[min_index])
```

in this function, the LI is: after iteration $i$, the first $i$ elements of `arr` will be sorted. for the inner loop, the LI is min_index is the index of the minimum element from `[i + 1, j]`

## contradiction
**disprove** the claim that $b$ and $p$ are positive integers such that $b$ is not a multiple of $p$, then $b^{p-1}$ 