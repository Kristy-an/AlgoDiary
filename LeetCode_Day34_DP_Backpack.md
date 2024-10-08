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





### [518. Coin Change II](https://leetcode.com/problems/coin-change-ii/)

A complete backpack problem.

Difference is each item can be take infinite times.

For 2-demension array approach, addjust `dp[i][j] = dp[i-1][j] + dp[i-1][j-coins[i]]` to `dp[i][j] = dp[i-1][j] + dp[i][j-coins[i]]`, therefore item `i` can be taken mutiple times.

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [[0]*(amount+1) for _ in range(len(coins))]

        # dp[i][j]: # of combin make up amount j with first i coins
        for list in dp:
            list[0] = 1
        
        for i in range(len(coins)):
            for j in range(1, amount+1):
                if coins[i]>amount: dp[i][j] = dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j] + dp[i][j-coins[i]]

        return dp[len(coins)-1][amount]
```

One array Approach:

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0]*(amount+1) 

        # dp[j]: # of combin make up amount j 
        dp[0] = 1

        for i in range(len(coins)):
            for j in range(coins[i], amount+1):
                dp[j] += dp[j-coins[i]]
        
        return dp[amount]
```



For solve combinations:

When using 2-demension array, you can switch the order of two `for` loop.

But when using 1-demension, you can't.

For solve max-value:

Can switch the order anyway.



### [377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/)

Name is Combination, but actually solve permutation.

Else, same as last question, but switch order of two `for` loop when using 1-demension array to change combination to permutation.

```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0]*(target+1) 
        dp[0] = 1

        for j in range(target+1):
            for i in range(len(nums)):
                if (j - nums[i] >= 0):
                    dp[j] += dp[j-nums[i]]
        return dp[target]
```





### [322. Coin Change](https://leetcode.com/problems/coin-change/)

Target: Min # of items in a combination/permutation 

Therefore it can be either c or p, order of for loops doesn't matter.

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')]*(amount+1) 

        # dp[i][j]: # of combin make up amount j with first i coins
        dp[0] = 0
        
        for i in range(len(coins)):
            for j in range(coins[i], amount+1):
                dp[j] = min(dp[j], dp[j-coins[i]]+1)

        return dp[amount] if dp[amount] != float('inf') else -1
```





[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/)

Clearly show why it's a same question with the previous one:

```python
class Solution:
    def numSquares(self, n: int) -> int:
        nums = [n*n for n in range(1,n+1)]

        dp = [float('inf')]*(n+1)
        dp[0] = 0

        for i in range(n):
            for j in range(nums[i], n+1):
                dp[j] = min(dp[j], dp[j-nums[i]]+1)
            print(dp)

        return dp[n] if dp[n] != float('inf') else -1
```

Clearer Code:

```python
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')]*(n+1)
        dp[0] = 0

        for i in range(1, int(n**0.5)+1):
            for j in range(i**2, n+1):
                dp[j] = min(dp[j], dp[j-i**2]+1)

        return dp[n]
```



### [139. Word Break](https://leetcode.com/problems/word-break/)

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        dp = [False] * (len(s)+1)
        dp[0] = True
            
        for j in range(1,len(s)+1):
            for word in wordDict:
                wordLen = len(word)
                if j >= wordLen and s[j-wordLen:j]==word:
                    if dp[j-wordLen]==True: dp[j]=True 
        
        return dp[len(s)]
```

