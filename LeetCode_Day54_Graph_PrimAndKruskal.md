# LeetCode Day 54 Graph Prim and Kruskal

Spanning Tree: A sub-graph of Graph G, which is also a tree includes all nodes in G.

Feature of spanning tree:

- Included all vertices
- It's a connected sub-graph. There is a path between any two vertices.
- No cycle
- Less edges (when including all vertices). Number of edges = Number of vertices - 1

Minimum Spanning Tree: The spanning tree of graph G with the smallest sum of edge weights.

Algorithm:

- Prim's Algorithm: Starting from a  vertex, gradually select the shortest edges connected to the already constructed tree until all vertices are included.
- Kruskal Algorithm: Based on the edge sorting and union-find data structure, edges are gradually added, and it is ensured that the selected edges do not form a cycle until a minimum spanning tree is constructed.

## Prim’s Algorithm

1. 第一步，选距离生成树最近节点
2. 第二步，最近节点加入生成树
3. 第三步，更新非生成树节点到生成树的距离（即更新minDist数组）



## Kruskal’s Algorithm
