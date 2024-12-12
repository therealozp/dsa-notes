goal is to find the subsets inside the array where the sum of subsets are is equal some pre-defined target.

for example, consider $n$ = 5, $W=21$, and given the items are `[5, 6, 10, 11, 16]`, the solutions are:
- `[5, 6, 10]`
- `[5, 16]`
- `[10, 11]`

this problem can be considered a reduction of the [[0-1 knapsack]] problem, where instead of having different profits per unit weight, all stolen items have the same profit/weight ratio, and the total needs to be exact.
## promising function
this problem can be solved by looping over all elements and checking the remaining, to apply backtracking and **prune** solutions, we need to do another preprocessing step: **sorting**,

with all weights organized nicely in a sorted array:
- consider our current capacity $w$. if we decide to take in another element $items[i]$, if $w + items[i] > W$, then all elements succeeding $i$ will also overload the weight. so, we prune this immediately.
- by the same token, we cannot have anything less than $W$. therefore, if adding all the weights of the remaining items **does not make it at least equal** to target, it will never be able to reach the result, aka $w + remaining < W$
