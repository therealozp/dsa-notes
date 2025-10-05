#dp

```python
def knapsack(value, weight, cap):
    def dp(i, remaining_capacity):
        if i >= len(value):
            return 0
            
        if remaining_capacity - weight[i] < 0:
            return dp(i + 1, remaining_capacity)
        
        skip = dp(i + 1, remaining_capacity)
        take = value[i] + dp(i + 1, remaining_capacity - weight[i])
        
        return max(skip, take)
        
    return dp(0, cap)

def bottomupknapsack(val, weight, cap):
    n = len(weight)
    dp = [[0] * (cap + 1) for _ in range(n)]
    for i in range(n):
        for w in range(cap + 1):
            if weight[i] > w:
                dp[i][w] = 0
                if i > 0:
                    dp[i][w] = dp[i - 1][w]
            else:
                skip = dp[i - 1][w]
                take = val[i] + dp[i - 1][w - weight[i]]
                dp[i][w] = max(skip, take) 
    
    for row in dp:
        print(row)
    return dp[-1][-1]
    
print(knapsack([3, 4, 5, 6], [2, 3, 4, 5],  5))
print(bottomupknapsack([3, 4, 5, 6], [2, 3, 4, 5],  5))
# print(bottomupknapsack([60, 100, 120],[10, 20, 30],50))
```