dac is a general algorithm design paradigm, consisting of three main steps: 
1. divide the input data `S` into two **disjoint** subsets `S1` and `S2`.
2. recur: solve the subproblems associated with S1 and S2. 
3. conquer: combine the solutions for s1 and s2 into solutions for `S`. 

note: the base case for the recursions are usually subproblems of size 0 or 1.