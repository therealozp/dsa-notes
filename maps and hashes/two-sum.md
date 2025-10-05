#arrays #hashmap 

```python
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hm = {}
        for i in range(len(nums)):
            if target - nums[i] in hm:
                return [hm[target-nums[i]], i]
            hm[nums[i]] = i
        return 0
```

