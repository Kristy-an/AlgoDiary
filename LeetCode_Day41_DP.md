# LeetCode Day 41 Dynamic Programming 

### [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

`dp[i]`:the length of the longest increasing subsequence that ends with the ith element.

**recurrence relation**

`dp[i] = max(dp[i], dp[j] + 1)`



```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1]*len(nums)
        for i in range(1, len(nums)):
            for j in range(0,i):
                if nums[j] < nums[i]:
                    dp[i] = max(dp[i], dp[j]+1)
        return max(dp)
```

Time:O(n^2)

Space: O(n)





### [674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)

Using Greedy is easier and better for this question.But good to think about the difference between this and last one when using DP.

Approach1: Greedy

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        res = 1
        count = 1

        for i in range(1, len(nums)):
            if nums[i] > nums[i-1]:
                count += 1
            else:
                count = 1
            res = max(res, count)
            
        return res
```

Approach2: DP

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        dp = [1] * len(nums)

        for i in range(1, len(nums)):
            if nums[i] > nums[i-1]:
                dp[i] = dp[i-1] + 1
        
        return max(dp)
```



### [718. Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/2021011215282060.jpg" alt="718.最长重复子数组" style="zoom:50%;" />

`dp[i][j]`: max len of repeated subarray of `nums1[:i-1]` and `nums2[:j-1]`. (end with `nums1[i-1]` or `nums2[j-1]`)

`i-1` and `j-1` for convenience. Could be `i` and `j`. But initialization will need to be done Seperetly.



```python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [[0]*(len(nums2)+1) for _ in range(len(nums1)+1)]
        res = 0
        for i in range(1, len(nums1) + 1):
            for j in range(1, len(nums2) + 1):
                if nums1[i-1]==nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                    res = max(res, dp[i][j])
        return res
```



### [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20210210150215918.jpg" alt="1143.最长公共子序列1" style="zoom:50%;" />

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        dp = [[0]*(len(text2)+1) for _ in range(len(text1)+1)]
        res = 0
        for i in range(1, len(text1) + 1):
            for j in range(1, len(text2) + 1):
                if text1[i-1]==text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])

        return dp[len(text1)][len(text2)]
```



### [1035. Uncrossed Lines](https://leetcode.com/problems/uncrossed-lines)

Exactly same with the LCS Problem.



### [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray)

Have been discussed in Greedy Part. Can also be done by using DP.

```python
class Solution:
     def maxSubArray(self, nums: List[int]) -> int:
        dp = [0]*len(nums)
        dp[0] = nums[0]
        res = dp[0]
        for i in range(1, len(nums)):
            dp[i] = max(dp[i-1] + nums[i], nums[i])
            if dp[i] > res:
                res = dp[i]
        return res
        
```



### [392. Is Subsequence](https://leetcode.com/problems/is-subsequence)

Approach 1: Two pointers

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        idxs = 0
        idxt = 0

        while idxs < len(s) and idxt < len(t):
            if s[idxs] == t[idxt]:
                idxs += 1
            idxt += 1

        return idxs == len(s)
```

Approach 2: DP

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/2021030317364166.jpg" alt="392.判断子序列2" style="zoom:50%;" />

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        dp = [[0]*(len(t)+1) for _ in range(len(s)+1)]

        for i in range(1, len(s)+1):
            for j in range(1, len(t)+1):
                if s[i-1]==t[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = dp[i][j-1]

        return dp[len(s)][len(t)] == len(s)
```



---



### [115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)



`dp[i][j]`: # of distinct subsequences of `s[:i-1]` which equals `t[:j-1]`

**Recurrence relation:**

`s[i-1] == t[i-1]`: `dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]`

`s[i-1] != t[i-1]`: `dp[i][j] = dp[i - 1][j]`

<img src="/Users/a/Library/Application Support/typora-user-images/image-20241014224536638.png" alt="image-20241014224536638" style="zoom:25%;" />

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        dp = [[0] * (len(t)+1) for _ in range(len(s)+1)]
        for i in range(len(s)+1):
            dp[i][0] = 1

        for i in range(1, len(s)+1):
            for j in range(1, len(t)+1):
                if s[i-1]==t[j-1]:
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j]

        return dp[len(s)][len(t)] 
```



### [583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/)

**Recurrence relation:**

- When `word[i-1] == word[j-1]`:

  - `dp[i][j]  = dp[i-1][j-1]`

- When  `word[i-1] != word[j-1]`:

  - Delete `word1[i-1]`, `dp[i-1][j]+1`
  - Delete `word2[j-1]`, `dp[i][j-1]+1`
  - Delete  `word1[i-1]` and  `word2[j-1]`, `dp[i - 1][j - 1] + 2`

  Therefore, `dp[i][j] = min({dp[i - 1][j - 1] + 2, dp[i - 1][j] + 1, dp[i][j - 1] + 1})`, 

  can be simplified to `dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1)`

**Initialization:**

When i is 0 (ie word1 is empty), we need delete j words (length of word2) to achieve the same as word1. Therefore, when i is 0, `dp[0][j]` is equal to j. Vice versa.

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]* (len(word2)+1) for _ in range(len(word1)+1)]

        for i in range(len(word1)+1):
            dp[i][0] = i
        for j in range(len(word2)+1):
            dp[0][j] = j

        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j]+1, dp[i][j-1]+1)
        return dp[len(word1)][len(word2)]
```



### [72. Edit Distance](https://leetcode.com/problems/edit-distance/)

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]* (len(word2)+1) for _ in range(len(word1)+1)]

        for i in range(len(word1)+1):
            dp[i][0] = i
        for j in range(len(word2)+1):
            dp[0][j] = j

        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1
                    
        return dp[len(word1)][len(word2)]
```

