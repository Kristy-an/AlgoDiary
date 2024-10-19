# LeetCode Day 48 Graph

## Basic Knowledge

### Type of Graph:

- Undirected graph/ directed graph
- Weighted graph
- Cyclic graph: A graph that contains at least one cycle
- Acyclic graph

### **Graph representation**

- Adjacency List
- Adjacency matrix

### Graph Traversal

- BFS
- DFS

### Graph Algorithms

- Dijkstra's : a variation of DFS and finds the shortest path between two nodes in a weighted graph.
- Bellman-Ford: a variation of BFS and finds the shortest paths in a weighted graph, even when negative edge weights are present.
- Floyd-Warshall: variation of BFS and finds the shortest paths between all pairs of nodes in a weighted graph.
- Topological sorting: It’s similar to DFS and orders nodes in a directed acyclic graph (DAG) to satisfy dependencies.
- Prim's: finds the minimum spanning tree of a connected, undirected graph.
- Kruskal's: finds the minimum spanning tree of a connected, undirected graph.

### Complexities of Graph Operations

| **Operation**               | **Adjacency List** | **Adjacency Matrix** |
| --------------------------- | ------------------ | -------------------- |
| *Add Vertex*                | O(1)               | O(V^2^)              |
| *Remove Vertex*             | O(V+E)             | O(V^2^)              |
| *Add Edge*                  | O(1)               | O(1)                 |
| *Remove Edge*               | O(E)               | O(1)                 |
| *Search*                    | O(V)               | O(1)                 |
| *Breadth First Search(BFS)* | O(V+E)             | O(V^2^)              |
| *Depth First Search(DFS)*   | O(V+E)             | O(V^2^)              |

If your application frequently manipulates vertices, the adjacency list is a better choice.

If you are dealing primarily with edges, the adjacency matrix is the more efficient approach.



## DFS: Bactracking

Pattern:

```python
def dfs(参数):
    if (终止条件):
        存放结果
        return

    for (选择：本节点所连接的其他节点):
        处理节点
        dfs(图，选择的节点) // 递归
        回溯，撤销处理结果
```



### [98. 所有可达路径](https://kamacoder.com/problempage.php?pid=1170)

### [797. All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)