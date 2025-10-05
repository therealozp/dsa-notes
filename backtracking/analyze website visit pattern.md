#backtracking

```python
def generate3sets(arr):
    if len(arr) < 3:
        return []
    res = []
    
    def backtrack(path, ind):
        if len(path) == 3:
            res.append(path.copy())
            return
        
        for i in range(ind + 1, len(arr)):
            path.append(arr[i])
            backtrack(path, i)
            path.pop()
    
    backtrack([], -1)    
    return res

def analyze(username, timestamp, website):
    visits = list(zip(username, timestamp, website))
    # sorts by user
    visits.sort()
    
    hm = defaultdict(list)
    for name, time, site in visits:
        hm[name].append(site)
    
    set_count = defaultdict(int)
    for name in hm:
        res = generate3sets(hm[name])
        print(res)
        if len(res) == 0:
            continue
        for s in res:
            set_count[tuple(s)] += 1
    
    print(set_count)
    return max(set_count.keys(), key=lambda x: set_count[x])
```
