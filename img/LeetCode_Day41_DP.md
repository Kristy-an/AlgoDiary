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

![718.最长重复子数组](https://code-thinking-1253855093.file.myqcloud.com/pics/2021011215282060.jpg)

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

