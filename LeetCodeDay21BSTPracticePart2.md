# LeetCode Day21 BST Practice Part 2

[669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

```python
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root:
            return None
        if root.val < low:
            return self.trimBST(root.right, low, high)
        elif root.val > high:
            return self.trimBST(root.left, low, high)
        root.left = self.trimBST(root.left, low, high)
        root.right = self.trimBST(root.right, low, high)
        return root
```



[108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

Easy

```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if len(nums)==0: return None
        if len(nums)==1: return TreeNode(nums[0])

        idx = len(nums)//2
        root = TreeNode(nums[idx])
        root.left = self.sortedArrayToBST(nums[0:idx])
        root.right = self.sortedArrayToBST(nums[idx+1:])
        return root
```

For better performance:

```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        root = self.traversal(nums, 0, len(nums)-1)
        return root

    def traversal(self, nums: List[int], left: int, right: int):
        if left>right: return None

        idx = (left+right)//2
        
        root = TreeNode(nums[idx])
        root.left = self.traversal(nums, left, idx-1)
        root.right = self.traversal(nums, idx+1, right)
        return root
```



[538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/)

```python
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        self.pre = 0 
        self.traversal(root)
        return root
        
    def traversal(self, cur):
        if cur is None:
            return        
        self.traversal(cur.right)
        cur.val += self.pre
        self.pre = cur.val
        self.traversal(cur.left)
```

