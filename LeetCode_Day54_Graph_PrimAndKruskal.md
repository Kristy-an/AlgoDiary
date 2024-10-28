# LeetCode Day 54 Graph Prim and Kruskal


[Introduction to Disjoint Set (Union-Find Algorithm)](https://www.geeksforgeeks.org/introduction-to-disjoint-set-data-structure-or-union-find-algorithm/)

Disjoint Set (Union-Find Algorithm)

*Two sets are called* ***\*disjoint sets\**** *if they don’t have any element in common*

A data structure that stores non overlapping or disjoint subset of elements is called disjoint set data structure. The disjoint set data structure supports following operations:

- Adding new sets to the disjoint set.
- Merging disjoint sets to a single disjoint set using ***\*Union\**** operation.
- Finding representative of a disjoint set using ***\*Find\**** operation.
- Check if two sets are disjoint or not. 



```python
class UnionFind:
    def __init__(self, n=1000):
        self.father = list(range(n))  # Initialize each node to be its own parent

    # Find the root of the set containing u, with path compression
    def find(self, u):
        if u != self.father[u]:
            self.father[u] = self.find(self.father[u])
        return self.father[u]

    # Check if two nodes are in the same set
    def is_connected(self, u, v):  # Fixed typo in method name
        return self.find(u) == self.find(v)

    # Join the sets containing u and v
    def join(self, u, v):
        root_u = self.find(u)
        root_v = self.find(v)
        if root_u != root_v:
            self.father[self.find(v)] = self.find(u)
```



Time Complexity: 

Find: O(log n) - O(1)



### [1971. Find if Path Exists in Graph](https://leetcode.com/problems/find-if-path-exists-in-graph)



### [684. Redundant Connection](https://leetcode.com/problems/redundant-connection/)





### [685. Redundant Connection II](https://leetcode.com/problems/redundant-connection-ii/)
