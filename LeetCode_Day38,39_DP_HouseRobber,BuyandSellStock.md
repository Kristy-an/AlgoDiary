# LeetCode Day 38 Dynamic Programming House Robber

### [198. House Robber](https://leetcode.com/problems/house-robber/)

`dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])`

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        dp = [0] * len(nums)
        dp[0] = nums[0]

        for i in range(1, len(nums)):
            if i == 1: 
                dp[i] = max(nums[0], nums[1])
            else:
                dp[i] = max(dp[i-1], dp[i-2]+nums[i])
        
        return dp[len(nums)-1]
```



### [213. House Robber II](https://leetcode.com/problems/house-robber-ii/)

Basiclly an application of problem 198. Use the function of 198 on nums[1:] and nums[:-1]

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 0 or nums is None:
            return 0

        if len(nums) == 1:
            return nums[0]

        return max(self.rob_simple(nums[:-1]), self.rob_simple(nums[1:]))

    def rob_simple(self, nums: List[int]) -> int:
        t1 = 0
        t2 = 0
        for current in nums:
            temp = t1
            t1 = max(current + t2, t1)
            t2 = temp

        return t1
```



### [337. House Robber III](https://leetcode.com/problems/house-robber-iii/)

```python
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        dp = self.traversal(root)
        return max(dp)

    def traversal(self, node):
        if not node:
            return (0, 0)

        left = self.traversal(node.left)
        right = self.traversal(node.right)

        val_0 = max(left) + max(right)
        val_1 = node.val + left[0] + right[0]

        return (val_0, val_1)
```



***



### [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)



#### Approach 1: Greedy

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        low = float("inf")
        result = 0
        for i in range(len(prices)):
            low = min(low, prices[i]) #取最左最小价格
            result = max(result, prices[i] - low) #直接取最大区间利润
        return result
```

![image-20241010224424430](/Users/a/Library/Application Support/typora-user-images/image-20241010224424430.png)

#### Approach 2: DP

##### DP List Define:

`dp[i][0]`: max $ if hold a stock on day i

`dp[i][0]`: max $ if hold no stock on day i

##### Recusive Relation:

`dp[i][0]` = `max(dp[i-1][0],-prices[i] )`

- `dp[i-1][0]`: hold a stack on `i-1`, remain the same on day i

- `-prices[i]`: buy a stack on day `i`

`dp[i][1]` = `max(dp[i-1][1], prices[i] + dp[i-1][0])`

- `dp[i-1][1]`: hold no stack on day `i-1`, remain the same

- `prices[i] + dp[i-1][0]`: throw the stack today (day `i`).

##### Initialization:

`dp[0][0]`: Because we hold a stock on day 0, therefore we must buy on day 0, so it will be `-prices[0]`

`dp[0][1]`: No stack on day 0, therefore 0.

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0,0] for _ in range(len(prices))]
        dp[0][0] = -prices[0]

        for i in range(1, len(prices)):
            dp[i][0] = max(dp[i-1][0], -prices[i])
            dp[i][1] = max(dp[i-1][1], dp[i][0]+prices[i])
        return dp[len(prices) - 1][1]
```

![121.买卖股票的最佳时机](https://code-thinking-1253855093.file.myqcloud.com/pics/20210224225642465.png)

Sum:

Using DP axtually do exactlly the same with greedy. we can see `dp[i][0]` maintain the min to buy, and `dp[i][1]` maintain the `max_profit`.



### [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

#### Approach 1: Greedy

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        result = 0

        for i in range(1, len(prices)):
            profit = prices[i] - prices[i-1]
            if profit > 0 :
                result += profit

        return result
```

#### Approach 2: DP

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0,0] for _ in range(len(prices))]
        dp[0][0] = -prices[0]

        for i in range(1, len(prices)):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i])
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i])
        return dp[len(prices) - 1][1]
```



### [123. Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)



```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        dp = [0] * 5 
        dp[1] = -prices[0]
        dp[3] = -prices[0]
        for i in range(1, len(prices)):
            dp[1] = max(dp[1], dp[0] - prices[i])
            dp[2] = max(dp[2], dp[1] + prices[i])
            dp[3] = max(dp[3], dp[2] - prices[i])
            dp[4] = max(dp[4], dp[3] + prices[i])
        return dp[4]
```

