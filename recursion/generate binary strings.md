## intuition
the binary strings will always be either 0 or 1, which allows us to define a base case (when the binary string has reached a certain size) and a recursive case (to add 0s and 1s)

## implementation

```python
def getBinStrings(index, string, size):
	# base case
	if index == size: 
		print(string)
	
	print(getBinStrings(index + 1, string + '0', size))
	print(getBinStrings(index + 1, string + '1', size))
	
```

