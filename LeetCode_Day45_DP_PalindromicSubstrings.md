# LeetCode Day 45 Dynamic Programming 

### [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)

#### Approach1: DP

1. Definition of dp table:

`dp[i][j]`: wether `s[i:j+1]` is a palindromic string. (1 if yes, 0 if no)

2. Recurrence relation

- `s[i] != s[j]` : `dp[i][j] = 0`

- `s[i] == s[j]` : 
  - If i equal to j, `dp[i][j] = 1`
  - if the difference between i and j is 1, `dp[i][j] = 1`
  - Else, need to see whether `dp[i+1][j-1]` is `1`. If so, one, if not, 0. 

3. Initialization: All 0

4. Traversal Order

   `dp[i][j]`  depends on `dp[i+1][j-1]`, therefore, `i` loop must be reversed.

   Or We can traverse j first and then i.

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        n = len(s)
        res = 0
        dp = [[0]*n for _ in range(n)]

        for i in range(n-1, -1, -1):
            for j in range(i, n):
                if s[i]==s[j]:
                    if j - i <= 1 or dp[i+1][j-1] == 1:
                        dp[i][j] = 1
                        res += 1
        return res
```

Time: O(n^2)

Space: O(n^2)

#### Approach2: Two Pointer

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        res = 0
        for i in range(len(s)):
            # s[i] as a center
            left = right = i
            while left >= 0 and right < len(s) and s[left]==s[right]:
                left -= 1
                right += 1
                res += 1
            
            # s[i] and s[i+1] as a center
            left = i 
            right = i + 1
            while left >= 0 and right < len(s) and s[left]==s[right]:
                left -= 1
                right += 1
                res += 1
        return res
```





### [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)



```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [[0]*n for _ in range(n)]

        for i in range(n-1, -1, -1):
            for j in range(i, n):
                if i==j:
                    dp[i][j] = 1
                    continue
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] + 2
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1])

        return dp[0][n-1]
```

