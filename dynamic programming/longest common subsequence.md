#dp #subsequences
 this problem makes use of the bottom-up dynamic programming approach, which helps reconstruct the bigger problem from smaller, subproblems. the problem statements asks to find the length of the longest common subsequence (lcs), for example: 

`abcde`
`ache`
-> `ace` is the longest common subsequence. 

to understand this, it often helps if we have a grid of elements. in this case, we have a 4x5 grid, which initialized as follows: 

![[longest common subsequence 2023-12-22 23.54.07.excalidraw|1000]]

here, the red arrows indicate when the remaining portion of the string does not match, whereas green means it is matching. this grid, suppose is called `dp`, will have contain the length of the **lcs between the two strings from that point**. Meaning, for example, at point `dp[0][0]`, it contains the same strings from the start, aka. `abcde` and `ache`. at point `dp[1][2]` for example, it would then be between `bcde` and `he`. 

## Intuition
It is important to develop an intuition here, is that the total greatest length of the lcs of the two strings lies at `[0][0]`. however, we understand that to get to the top, we need to evaluate the two strings *from the end*, so that we can calculate the remainder of the strings from there. for example, `e` and `e` both matches `1`, `he` and `e` matches 1, `de` and `e` matches 1, and so on. 

the reason for the direction of the arrows is that, when we find a matching character, we want to advance to the next character of **both** strings, hence `dp[i+1][j+1]`. when it does not match, we only want to check from the next point of each string separately, meaning `dp[i+1][j]` and `dp[i][j+1]`. 

therefore, to reconstruct the longest length, we start from the bottom, and work our way back up. notice that there are only 2 cases to check for, if the character at that point matches, or it does not. if it matches, we simply regress to `dp[i - 1][j - 1]`, else, we consider the **maximum** value between `dp[i - 1][j]` or `dp[i][j - 1]`. 


```python
def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        dp = [[0 for _ in range(len(text1) + 1) ] for _ in range(len(text2) + 1)]
        
        for i in range(len(text2) - 1, -1, -1):
            for j in range(len(text1) -1, -1, -1):
                if text2[i] == text1[j]:
                    dp[i][j] = 1 + dp[i + 1][j + 1]
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j + 1])
        return dp[0][0]
```

## Complexity
Time complexity: $O(m \times n)$, where $m$ and $n$ are the lengths of each string
Space complexity: $O(m \times n)$, as we have to store a 2D array.