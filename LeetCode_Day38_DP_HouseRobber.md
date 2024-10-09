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

