# LeetCode Day27,28 Greedy



[Greedy Algorithms](https://www.geeksforgeeks.org/greedy-algorithms/)

***Greedy algorithms*** are a class of algorithms that make ***locally optimal*** choices at each step with the hope of finding a ***global optimum*** solution. In these algorithms, decisions are made based on the information available at the <u>current moment</u> without considering the consequences of these decisions in the <u>future</u>. 

The key idea is to <u>select the best possible choice at each step</u>, leading to a solution that may not always be the most optimal but is often good enough for many problems.



### [455. Assign Cookies](https://leetcode.com/problems/assign-cookies/)

```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        count = 0

        gidx = len(g) - 1
        sidx = len(s) - 1

        while gidx>=0 and sidx>=0:
            if g[gidx] <= s[sidx]:
                count += 1
                gidx -= 1
                sidx -= 1
            else:
                gidx -= 1
        return count
```



### [376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/)

Count the number of peaks in the array (equivalent to deleting nodes on a single slope and then counting the length)

##### Approach 1:

Simply count th peaks of the array, need to consider all conditions, like the start and end of the array, and the condition when there are several countinous same numbers in the array. 

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return 1

        leftdiff = 0
        leftidx = 0
        rightdiff = 0

        count = 1
        for i in range(0, len(nums)-1):
            rightdiff = nums[i]-nums[i+1]
            if rightdiff == 0:
                continue
            leftdiff = nums[i]-nums[leftidx]
            if leftdiff*rightdiff >= 0:
                count += 1
            leftidx = i

        return count
```

Time Complexity: O(n)

##### Approach 2: Dynamic Programming

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        dp = [ [0]*2 for i in range(len(nums))]
        dp[0][0] = dp[0][1] = 1
        for i in range(len(nums)):
            dp[i][0] = dp[i][1] = 1
            for j in range(i):
                if nums[j] > nums[i]:
                    dp[i][1] = max(dp[i][1], dp[j][0]+1)
            for j in range(i):
                if nums[j] < nums[i]:
                    dp[i][0] = max(dp[i][0], dp[j][1]+1)
            print(dp)
        return max(dp[len(nums)-1][0], dp[len(nums)-1][1])
```



Note:

When initialize a 2-demision list in python, we can not do `[[0]*n]*m`, because in this way, we are creating `m` reference to list `[0]*n`. Therefore, all m list will change if we change anyone of them.

Instead, we can use List Comprehension:

`dp = [ [0]*n for i in range(m)]`

`dp = [[0 for i in range(n)] for j in range(m)]`

Or `numpy`

```python
import numpy as np
test = np.zeros((m, n))
```



### [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

##### Approach 1: Greedy

Abandon when sum is smaller than 0.

```python
class Solution:
     def maxSubArray(self, nums: List[int]) -> int:
        max = -10000
        sum = 0
        for i in range(len(nums)):
            sum += nums[i]
            if sum > max: max = sum
            if sum <= 0 :
                sum = 0
            
        return max
```

O(n), O(1)

##### Approach 2: Dynamic Programming

 dp[i] = max(dp[i - 1] + nums[i], nums[i])

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

O(n), O(n)



### [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

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



55 ask whether the destination can be reached, 45 asks the minimum step we need to walk to reach the destination. Therefore 45 is a little harder than 55.

For 55, just calculate whether the end will be covered.

For 45, we will need an additional iteration to count the farest point we can reach each time.

### [55. Jump Game](https://leetcode.com/problems/jump-game/)

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        if len(nums) == 1: return True
        cover = 0
        i = 0
        while i <= cover:
            cover = max(i + nums[i], cover)
            if cover >= len(nums) - 1: return True
            i += 1
        return False
```



### [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        if len(nums)==1:
            return 0
        far = 0
        count = 1
        cover_left = 0
        cover_right = far = nums[0]

        while far < len(nums)-1:
            count += 1
            for i in range(cover_left, cover_right+1):
                far = max(far, i + nums[i])
            cover_left = cover_right+1
            cover_right = far
        return count
```



### [1005. Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/)

```python
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        nums.sort()

        for i in range(k):
            if i<len(nums) and nums[i] < 0: 
                nums[i] = -nums[i]
            else:
                nums.sort()
                nums[0] = -nums[0]   
        return sum(nums)     
```

