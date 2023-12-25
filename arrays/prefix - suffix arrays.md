in dsa, using prefix or suffix arrays of a sum or a product can help reduce speeds from $O(n^2)$ to $O(n)$. the strategy is that we pre-compute the accumulating sums/products of each element in the array, and then using those to compute whatever the problem requires. 

put simply, the prefix array is the product/sum of elements **before** a certain index, while a pthe suffix counterpart is simply the **latter**. hence, we can derive quickly that without a certain index means we can just pair the two together.

for example, if the problem to asks to compute the product of each element in the array **except** for the current element, we can implement this by using: 

```python
def productExceptSelf(nums: List[int]) -> List[int]:
	prefix = [1]
	suffix = [1]
	for i in range(len(nums)):
		prefix.append(prefix[-1] * nums[i])
		suffix.append(suffix[-1] * nums[len(nums) - i - 1])
		
	suffix.reverse()
	fin = []
	
	for i in range(len(nums)):
		fin.append(prefix[i] * suffix[i + 1]
	return fin
```

for example, in the case of `[1, 2, 3, 4]`, the prefix array will be `[1, 1, 2, 6, 24]`and the suffix array will become `[1, 4, 12, 24, 24]` (1 is added to help with cleaner code). after a reverse, we can promptly calculate the prefix-suffix sums / products. 

for this problem specifically, we can optimize the space complexity even further, reducing it from $O(n)$ to $O(1)$ by using the output array as the prefix-suffix array itself. below is the implementation: 

```python
def productExceptSelf(nums):
    n = len(nums)    
    output = [1] * n
    left_product = 1
    for i in range(n):
        output[i] *= left_product
        left_product *= nums[i]
        
    right_product = 1
    for i in range(n - 1, -1, -1):
        output[i] *= right_product
        right_product *= nums[i]
        
	return output
```
