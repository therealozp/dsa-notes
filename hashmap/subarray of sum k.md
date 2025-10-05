## key insights
for any array `a` with a prefix sum of `pref`, a subarray of `a` from indices `i` to `j` will be `pref[j] - pref[i]`. this way, we can simply use `k` to create keys where we can optimize this.

```python
class Solution(object):
    def subarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """

        c = 0

        pref = [nums[0]]
        for i in range(1, len(nums)):
            pref.append(pref[-1] + nums[i])

        seen_count = defaultdict(int)
        for i in range(len(nums)):
            if pref[i] == k:
                c += 1

            to_reach = pref[i] - k
            c += seen_count[to_reach]

            seen_count[pref[i]] += 1

        return c
```

