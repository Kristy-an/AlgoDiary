# LeetCode Day 55 Graph Topological Sorting

##  Topological Sorting

Steps:
1. Find the node with in-degree 0 and add it to the result set
2. Remove the node from the graph

Repeat the above two steps until all nodes are removed from the graph.

The order of the result set is the topological sorting order we want (the order in the result set may not be unique)

If we can not find any more node with in-degree 0, Then if we find that the number of elements in the result set is not equal to the number of nodes in the graph, we can conclude that there must be a directed cycle in the graph!

```python
from collections import deque 

v, e = map(int, input().split())

umap = {}
inDegree = [0]*v
res = []

for _  in range(e):
    v1, v2 = map(int, input().split())
    inDegree[v2] += 1
    umap[v1] = umap.get(v1, [])
    umap[v1].append(v2)

que = deque([])
for i in range(v):
    if inDegree[i]==0:
        que.append(i)

while que:
    cur = que.popleft()
    res.append(cur)
    
    nodesPointed = umap.get(cur, [])

    for node in nodesPointed:
        inDegree[node] -= 1
        if inDegree[node] == 0:
            que.append(node)

if len(res) == v:
    print(" ".join(map(str,res)))
else:
    print(-1)
```
