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



A good article for understanding "from bottom to top", "optimal sub-structure", "Memoization Search（memorandum“. But example not hard enough. https://www.cxyxiaowu.com/8536.html



[1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

Actually similar with question 416. After figuring out our aim is seperate the stones to two parts with minimum difference of their total weights.

```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        target = sum(stones)//2

        dp = [ [0]*(target+1) for _ in range(len(stones))]

        # dp[i][j]: The max total weight when we have 0-i stones
        # and weight limit is `target`
        for i in range(len(stones)):
            for j in range(target+1):
                if stones[i] > j: dp[i][j] = dp[i-1][j]
                else: dp[i][j] = max(dp[i-1][j], dp[i-1][j-stones[i]] + stones[i])

        return sum(stones) - 2*dp[len(stones)-1][target]
```



### [494. Target Sum](https://leetcode.com/problems/target-sum/)

Before, we calculate the max value the bag can have

This time, we want the number of ways we can fill the bag. This is a combination problem. 

#### Formulate state and transition relationships：

If current number `nums[i]` bigger than bagsize `j`, then the number of ways we can fill the bag of size `j` will be equal to the number of ways when using 0 - (i-1) item, because `nums[i]` can not be used anyway.

Therefore,  `dp[i][j] = dp[i-1][j]`

Else, the number of ways we can fill the bag of size `j` will be equal to:

1. `dp[i-1][j]`: the number of ways when using 0 - (i-1) item plus 
2. `dp[i-1][j-nums[i]]`:  the number of ways when using 0 - (i-1) item to fill a bag with bagsize `j-nums[i]`

Because we can achieve current state by whether using or not using item i. Not using item i corresponding to situation 1, using item i corresponding to situation 2. Therefore the total ways to fill the bag will be the sum of these two situations.

Therefore,  `dp[i][j] = dp[i-1][j] + dp[i-1][j-nums[i]]`



```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        # For each num in nums, we either give it + or -
        # Separate all nums into two parts, one part assign +, another assign -
        # part 1 - part 2 = target
        # part 1 + part 2 = sum(nums)
        # Therefore, part 1 = (target+sum(nums))/2

        if (target + sum(nums)) % 2 == 1 or target + sum(nums) < 0:
            return 0
        bagsize = (target + sum(nums)) // 2

        # dp[i][j]: ways to using 0-i nums to fill bag with size j
        dp = [[0]*(bagsize+1) for _ in range(len(nums))]
        
        # Initialization of dp
        if nums[0] <= bagsize: dp[0][nums[0]] = 1

        dp[0][0] = 1 if nums[0]!=0 else 2

        for i in range(1, len(nums)):
            for j in range(bagsize+1):
                if nums[i]>j: dp[i][j] = dp[i-1][j]
                else: dp[i][j] = dp[i-1][j] + dp[i-1][j-nums[i]]

        return dp[len(nums)-1][bagsize]

```



Initialization can be simplified from:

```python
        if nums[0] <= bagsize: dp[0][nums[0]] = 1
        dp[0][0] = 1 if nums[0]!=0 else 2
```

To:

```python
        dp[0][0] = 1
    		if nums[0] <= bagsize: dp[0][nums[0]] += 1
```



Only use one demision list: 

Remember the order of traversal of `bagsize` need to be reversed.

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:

        if (target + sum(nums)) % 2 == 1 or target + sum(nums) < 0:
            return 0
        bagsize = (target + sum(nums)) // 2

        dp = [0]*(bagsize+1)
        dp[0] = 1
        if nums[0] <= bagsize: dp[nums[0]] += 1

        for i in range(1, len(nums)):
            for j in range(bagsize,nums[i]-1, -1):
                dp[j] += dp[j-nums[i]]

        return dp[bagsize]


```





