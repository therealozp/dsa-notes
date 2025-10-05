find all unique permutations, given an original string. for example, if string is `AAB`, the unique combos are `[AAB, ABA, BAA]`.
## state-space tree
should contain all the unique possibilities of strings we can select. this is so that we can avoid duplicate paths at run time. for example. on the string `AAB`, we can go:
- A (0) -> A (1) -> B (valid permutation)
- A (1) duplicate of the first path, because we have seen this iteration before.  
## promising function
the promising function checks three things:
- the current character must not be used, and either
	- it is the first character
	- it is not a duplicate of the last character
	- the last character is used (to handle the consecutive character being included in the final string, otherwise, iterating A -> A would not work)

```python
def promising(idx, used, s):
    if used[idx]:
        return False
    if idx == 0 or s[idx] != s[idx - 1] or used[idx - 1]:
        return True
    return False

def backtrack(s, used, curr_string=[]):
    if len(curr_string) == len(s):
        print("".join(curr_string))
        return
    
    for i in range(len(s)):
        if promising(i, used, s):
            used[i] = True
            curr_string.append(s[i])
            
            backtrack(s, used, curr_string)
            
            used[i] = False
            curr_string.pop()

s = "aba"
s = sorted(s)  # Sort to handle duplicates properly
used = [False] * len(s)
backtrack(s, used)
```

## time complexity
all permutations of a string takes $O(n!)$ to compute and display.

