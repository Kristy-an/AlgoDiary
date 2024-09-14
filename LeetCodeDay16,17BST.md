# LeetCode Day16,17  Binary Search Tree Practice



###### [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

```python
class Solution:
    def __init__(self):
        self.max = (-2)**31 - 1

    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        left = self.isValidBST(root.left)

        if self.max<root.val:
            self.max = root.val
        else:
            return False
        right = self.isValidBST(root.right)
        return left and right
```



###### [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

For this problem, we are using recursion as a method of traversal instead of recursive solve the minimum difference. Because the order of the in-order travesal is just the sorted BST. 

Therefore we do not need a return value for the recursive function. Instead, `minidiff` will be stored in class attribute and update in each calculation during traversal. 

```python
class Solution:
    def __init__(self):
        self.minidiff = float('inf')  # Use infinity for large value
        self.pre = None

    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        if not root:
            return self.minidiff

        self.getMinimumDifference(root.left)

        if self.pre:
            self.minidiff = min(self.minidiff, abs(root.val - self.pre.val))
        
        self.pre = root

        self.getMinimumDifference(root.right)
        
        return self.minidiff
```



###### [501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/)

```python
class Solution:
    def __init__(self):
        self.count = 0
        self.maxCount = 0
        self.pre = None
        self.res = []
        
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return self.res
        self.findMode(root.left)

        if not self.pre:
            self.count = 1
        elif self.pre.val == root.val:
            self.count += 1
        else:
            self.count = 1
        self.pre = root

        if self.count == self.maxCount:
            self.res.append(root.val)
        if self.count > self.maxCount:
            self.maxCount = self.count
            self.res = []
            self.res.append(root.val)
        
        self.findMode(root.right)
        return self.res
```

I think for this problem, it's better to seperate the traversal function and the solution function. Because we are not actually using recursion to solve the max count. 

The core logic for finding the modes happens during this traversal by comparing the current node's value with the previous node and adjusting the count accordingly.

```python
class Solution:
    def __init__(self):
        self.maxCount = 0 
        self.count = 0
        self.pre = None
        self.result = []

    def searchBST(self, cur):
        if cur is None:
            return

        self.searchBST(cur.left)

        if self.pre is None:
            self.count = 1
        elif self.pre.val == cur.val: 
            self.count += 1
        else:
            self.count = 1
        self.pre = cur

        if self.count == self.maxCount:
            self.result.append(cur.val)

        if self.count > self.maxCount: 
            self.maxCount = self.count
            self.result = [cur.val]

        self.searchBST(cur.right)
        return

    def findMode(self, root):
        self.searchBST(root)
        return self.result
```



```python
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        count = 0
        maxCount = 0
        pre = None
        
        def searchBST(root: Optional[TreeNode]) -> List[int]:
            nonlocal count, maxCount, pre, res
            if not root:
                return
            searchBST(root.left)

            if not pre:
                count = 1
            elif pre.val == root.val:
                count += 1
            else:
                count = 1
            pre = root

            if count == maxCount:
                res.append(root.val)
            if count > maxCount:
                maxCount = count
                res = [root.val]
            
            searchBST(root.right)

        searchBST(root)
        return res
```



###### [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if root == q or root == p or root is None:
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left is not None and right is not None:
            return root

        if left is None and right is not None:
            return right
        elif left is not None and right is None:
            return left
        else: 
            return None
```

