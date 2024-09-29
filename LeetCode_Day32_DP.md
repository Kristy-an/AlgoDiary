# LeetCode Day 32 Dynamic Programming

Dynamic Programming Step:

1. Determine the dp array and the meaning of it's index
2. Determine the induction fomular
3. Initialize the dp array
4. Determine the order of traversal
5. Make example and try deduct the dp array



### [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

Approach 1: DP

```python
class Solution:
    def fib(self, n: int) -> int:
        if n <= 1: return n
        dp = [0]*(n+1)
        dp[0] = 0
        dp[1] = 1
        for i in range(2, n+1):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[n]
```

Time Complexity: O(n)

Approach 2: Recursion

```python
class Solution:
    def fib(self, n: int) -> int:
        if n==0: return 0
        if n==1: return 1

        return self.fib(n-1)+self.fib(n-2)
```

Time Complexity: O(2^n)

### [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1 :
            return 1

        dp = [0]*(n+1)
        dp[1] = 1
        dp[2] = 2
        for x in range(3, n+1):
            dp[x] = dp[x-2] + dp[x-1]
        return dp[n]
```

### [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        dp = [0] * len(cost)
        dp[0] = cost[0]
        dp[1] = cost[1]
        for x in range(2, len(cost)):
            dp[x] = min(dp[x-1], dp[x-2]) + cost[x]
        return min(dp[len(cost)-1], dp[len(cost)-2])
```

