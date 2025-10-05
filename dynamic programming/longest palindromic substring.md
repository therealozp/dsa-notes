#dp

instead of looping over individual substrings and then determining if the substrings are palindromic (which should cost $O(n^3)$ in the worst case), we leverage a key insight: **a palindromic string has a palindromic substring.**

so to speak, consider the string `babbac`:
- we can visually determine that the LPS is `abba`, and within that `abba`, we recognize that `bb` is also another palindromic substring.
- so, instead of manually looping over each substring and checking, we **expand outwards** from a certain index, checking continuously if the expansion is valid.

```python
def longestPalindrome(self, s: str) -> str:
	lps = s[0]
	for i in range(len(s)):
		# check odd-length palindromic substrs
		l = r = i
		while l >= 0 and r < len(s):
			if s[l] == s[r]:
				l -= 1
				r += 1
			else:
				break
		# here, it breaks
		if r - l - 1 > len(lps):
			lps = s[l+1:r]
		
		# check even-length palindromic substrs
		l = i
		r = i + 1
		while l >= 0 and r < len(s):
			if s[l] == s[r]:
				l -= 1
				r += 1
			else:
				break
		# here, it breaks
		if r - l - 1 > len(lps):
			lps = s[l+1:r]
	return lps
```
