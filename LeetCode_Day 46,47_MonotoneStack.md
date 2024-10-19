# LeetCode Day 46,47 **Monotone Stack**



**Monotone Stack**: A stack that the elements in the stack are strictly increasing (or decreasing).

**How to implement?** 

- Each time an element come in, pop out all elements in the stack which smaller than the current element. Then push the element in. Therefore, the elements will always decreasing in the stack. 

**Example:** Given an arrray `nums`, return an array, on each index, it store the corresponding next bigger element of the `nums` array, store `-1` if no bigger element found.

`nums = [2,1,2,4,3]`

`res = [4,2,4,-1,-1]`

Broute-Force will result in O(n^2) Time complexity.

**Solution:** Iterate the `nums` array from end to start, 

```python
def calculateGreaterElement(nums):
    n = len(nums)
    res = [0]*n
    
    s = []
    for i in range(n-1, -1, -1):
        while s and s[-1] <= nums[i]:
            s.pop()
        res[i] = -1 if not s else s[-1]
        s.append(nums[i])
    return res
```

![](img/ms1.jpg)



[739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures)



```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)
        res = [0]*n
    
        s = [0]
        for i in range(1, n):
            if temperatures[i] <= temperatures[s[-1]]:
                s.append(i)
            else:
                while s and temperatures[i] > temperatures[s[-1]]:
                    res[s[-1]] = i - s[-1]
                    s.pop()
                s.append(i)
        return res
```



[496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i)



[503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii)



[42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water)



[84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram)