# LeetCode Day19 BST Practice

[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

Compared with [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/), this problem will be easier if we can utilize the feature of BST. The ancestor of `p` and `q` must be in the interval of [p,q] (or [q,p]).

As you traverse from top to bottom, if the first node encountered has a value within the interval [p, q], then the current node (cur) can be identified as the common ancestor of p and q. Furthermore, this node is guaranteed to be the lowest common ancestor (proof omitted).

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root:
            return None
        
        if root.val<=max(q.val,p.val) and root.val>=min(q.val,p.val):
            return root
        elif root.val > max(q.val,p.val):
            return self.lowestCommonAncestor(root.left, p, q)
        else:
            return self.lowestCommonAncestor(root.right, p, q)
```

```python
# Cleaner the Code
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root
```

Iteration:

```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        while root:
            if root.val > p.val and root.val > q.val:
                root = root.left
            elif root.val < p.val and root.val < q.val:
                root = root.right
            else:
                return root
        return None
```



[701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)

Nothing special, just search until a `None` node then insert.

Iteration:

```python
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        node = TreeNode(val)

        if not root:
            return node 

        cur = root
        parent = None
        while cur:
            parent = cur
            if val < cur.val:
                cur = cur.left
            else:
                cur = cur.right
        
        if val < parent.val:
            parent.left = node 
        else:
            parent.right = node 
            
        return root
```



Recursion:

```python
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return TreeNode(val)

        if val < root.val:
            root.left = self.insertIntoBST(root.left, val)
        else:
            root.right = self.insertIntoBST(root.right, val)
            
        return root
```



[450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)

```python
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return None

        if key < root.val:
            root.left = self.deleteNode(root.left, key)
        elif key > root.val:
            root.right = self.deleteNode(root.right, key)
        elif key == root.val:
            if not root.left and not root.right:
                return None
            elif root.left and root.right:
                cur = root.left
                while cur.right:
                    cur = cur.right
                root.val = cur.val
                root.left = self.deleteNode(root.left, cur.val)
            elif not root.left:
                root = root.right
            elif not root.right:
                root = root.left
        return root
```

Mistake I made:

```python
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return None
        if key < root.val:
            root.left = self.deleteNode(root.left, key)
        if key > root.val:
            root.right = self.deleteNode(root.right, key)
        if key == root.val:
            if not root.left and not root.right:
                return None
            if root.left and root.right:
                cur = root.left
                while cur.right:
                    cur = cur.right
                root.val = cur.val
                cur = None
                return root
            if not root.left:
                root = root.right
            if not root.right:
                root = root.left
        return root
```

`cur = None` can not really delete the Node. If only make `cur` point to a `None`, however, self.left is still point the "real" node.