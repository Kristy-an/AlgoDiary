# LeetCode Day29,30 Greedy Part 2



### [134. Gas Station](https://leetcode.com/problems/gas-station/)

Approach 1: Broute Force (Time Limits Exceeded)

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        remain = [a-b for a,b in zip(gas, cost)]

        for i in range(len(gas)):
            sum = 0
            for j in range(len(gas)):
                sum += remain[(i+j)%len(gas)]
                if sum < 0:
                    break
                if j == len(gas) - 1:
                    return i 
        return -1
```



Approach 2: Greedy

What we can deduce:

1. If the sum of `gas` is smaller than the sum of `cost`, we can not finish a circular route anyway.
2. If the sum of `gas` is not smaller than the sum of `cost`, we must can find a start able to finish  a circular route.

3. Calculate from the start, if our remain gas (`cur_sum`) is smaller than zero, then this start station is not a solution. **But besides this**, we can know **all stations form the start station to current station** won't be a solution. 

   Therefore, we can start from the next gas station of current station. Do this until the end of the for loop, we can must find a solution (because we have already verify there must be a solution by 2).

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        remain = [a-b for a,b in zip(gas, cost)]
        if sum(remain)<0:
            return -1

        start = 0
        cur_sum = 0
        for i in range(len(gas)):
            cur_sum += remain[i]

            if cur_sum < 0:
                start = i + 1
                cur_sum = 0
        
        return start
```



### [135. Candy](https://leetcode.com/problems/candy/)

The number of candies for each child needs to take into account the scores of the children on both sides, so it is difficult to solve this problem by only traversing from front to back. This is the mistake I made. I spent an hour trying to observe the monotonicity law to solve the problem, constantly correcting it, and finally found that I still missed a situation. 

Because it needs to be compared with the "neighbors", the comparison of each element is "bidirectional", so traversing in one direction cannot solve the problem.

![painfulexperience](img/painfulexperience.jpg)

But we can easily get the answer if we traverse the list from head to end and then end to head!

```python
class Solution:
    def candy(self, ratings: List[int]) -> int:
        start = 0
        candy = [1]*len(ratings)

        for i in range(1, len(ratings)):
            if ratings[i]>ratings[i-1]:
                candy[i] = candy[i-1] + 1
        
        for i in range(len(ratings)-1, 0, -1):
            if ratings[i-1]>ratings[i] and candy[i-1]<=candy[i]:
                candy[i-1] = candy[i] + 1
                
        return sum(candy)
```

Note:

If you want to use for loop with decreasing index, use `range(a,  b, -1)`. But `a` is bigger than `b`. Still obay the left-close right-open convention.

Time Complexity: O(n)

Space Complexity: O(n)

Note: More solution on Leetcode, with Time Complexity O(n) and Space Complexity O(1). <mark>Check back</mark>

https://leetcode.com/problems/candy/editorial/



### [860. Lemonade Change](https://leetcode.com/problems/lemonade-change/)

Easy, skip.

### [406. Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/)

妙

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda x: (-x[0], x[1]))
        res = []
 
        for p in people:
            res.insert(p[1], p)
        return res
```



## Overlapping intervals problem

### [452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        points.sort(key = lambda x: x[0])
        res = 1

        for i in range(1, len(points)):
            if points[i][0] > points[i-1][1]:
                res += 1
            else:
                # Update the min right boundary of the ballons
                points[i][1] = min(points[i-1][1], points[i][1])
        return res
```

Time: O(nlogn)

Space: O(1)

### [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

The root logic is, if we meet two overlap intervals, we will delete the one with bigger right boundary, which will better prevent future overlap. (greedy)

But we don't really do deletion, but stimulate it by maintain a `right` variable, represent the biggest right boundary after deletion. 

If detecte overlap, after delete the one with bigger right boundary, over newer biggest right boundary will be `right = min(right, intervals[i][1])`.

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key = lambda x:x[0])
        res = 0
        right = 0

        for i in range(1, len(intervals)):
            if intervals[i][0] < intervals[i-1][1]:
                res += 1
                intervals[i][1] = min(intervals[i-1][1], intervals[i][1])

        return res
```

Do not change the list:

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key = lambda x:x[0])
        res = 0
        right = intervals[0][1]

        for i in range(1, len(intervals)):
            print(intervals[i])
            if intervals[i][0] < right:
                res += 1
                right = min(right, intervals[i][1])
            else:
                right = intervals[i][1]

        return res
```



### [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)



```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key = lambda x:x[0])
        res = []

        right = intervals[0][1]
        res.append(intervals[0])

        for i in range(1, len(intervals)):
            if intervals[i][0] <= right:
                right = max(right, intervals[i][1])
                res[-1][1] = right
            else:
                res.append(intervals[i])
                right = intervals[i][1]
        return res
```



## More

### [736. Parse Lisp Expression](https://leetcode.com/problems/parse-lisp-expression/)

```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        last_letter_position = [0]*26
        for i in range(len(s)):
            last_letter_position[ord(s[i])-ord('a')] = i

        left = right = 0
        res = []
        for i in range(len(s)):

            right =  max(right, last_letter_position[ord(s[i])-ord('a')])
            if i == right:
                res.append(right - left + 1)
                left = right + 1
        return res
```

Note:

Use `ord()` to get the Unicode code point or ASCII value of a single character. Convienently trans 26 letters to integer 0-25



### [738. Monotone Increasing Digits](https://leetcode.com/problems/monotone-increasing-digits/)

```python
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        number = list(str(n))
        flag = len(number)
        for i in range(len(number)-1, 0, -1):
            if number[i-1]>number[i]:
                flag = i
                number[i-1] = str(int(number[i-1])-1)
        for i in range(flag, len(number)):
            number[i] = '9'
        return int(''.join(number))
```



### [968. Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

First place a camera at the parent node of the leaf node, then place a camera every two nodes until you reach the root node of the binary tree.

0: this node hasn't been cover

1: this node has a camera

2: this code is covered (no camera)

We need to use **post-order traversal** to make sure we deduct from bottom to top.

```python
class Solution:
    def minCameraCover(self, root: Optional[TreeNode]) -> int:
        self.res = 0
        if self.traversal(root)==0:
            self.res += 1
        return self.res
        
    def traversal(self, cur):
        if cur==None:
            return 2
        left = self.traversal(cur.left)
        right = self.traversal(cur.right)
        
        # If both childen nodes are covered, means this node hasn't been covered
        if left==2 and right==2:
            return 0
        # If one of the children node hasn't been covered. We need a camera on this node
        if left==0 or right==0:
            self.res += 1
            return 1
        # If any children node has camera, then it means this node is covered
        if left==1 or right ==1:
            return 2
```

