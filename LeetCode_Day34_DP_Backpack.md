# LeetCode Day 34 Dynamic Programming Backpack Problem



### [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        if sum(nums)%2==1:
            return False
        target = sum(nums)//2

        dp = [[0]*(target+1) for m in range(len(nums))]

        for j in range(target):
            if j >= nums[0]:
                dp[0][j] = nums[0]

        for i in range(1, len(nums)):
            for j in range(target+1):
                if j < nums[i]: dp[i][j] = dp[i-1][j]
                else: dp[i][j] = max(dp[i-1][j], dp[i-1][j-nums[i]] + nums[i])
        
        if dp[len(nums)-1][target] == target:
            return True
        else:
            return False
```

