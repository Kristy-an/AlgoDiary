# LeetCode Day14 Binary Tree Practice

Nearly all problems of Binary Tree can be solved with the logic of traversal. Either pre-order, in-order, post-order or level order. 

And all traversal methods can be implemented recusively or iteratively. It's nice practice to think about whether other approaches are feasible while we using the most understandable method to solve the problem.



[226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```

[101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        return self.compare(root.left, root.right)
    
    def compare(self, left, right):
        if left and right and left.val==right.val and self.compare(left.left, right.right) and self.compare(left.right, right.left):
            return True
        if not left and not right:
            return True
        return False
```



[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

Recursively get the maxDepth of current node' sub-trees. Pick the bigger depth and add one to get the current node's maxDepth.

Recall the depth and height of a tree, depth count from top(root) to bottom(leaf), height count from bottom to top. 

![](./img/tree_heightanddepth.jpg)

Root node has depth of 0, but it's height is the max depth of the whole tree. Therefore, we are also solving the height of root. 

All nodes on the same level have the same depth, but not always have the same height, because head count from leaf.

Under the code, there's the logic of post-order, because we first need to know the maxDepth of left sub-tree and right sub-tree, pick the bigger one, then finally calculate for the root node.

```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right))+1
```

So, when solving the height of root, we use post-order traversal, paired with the next question, let's see what should we use. 

[111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

Recursively get the minimum height of root's sub-trees. However, what if the node only have one subtree? We can not simple pick the minimun which is 0!

```python
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        if root.left and not root.right:
            return self.minDepth(root.left) + 1
        if root.right and not root.left:
            return self.minDepth(root.right) + 1
        left_depth = self.minDepth(root.left)
        right_depth = self.minDepth(root.right)
        return min(left_depth, right_depth)+1
        
```

