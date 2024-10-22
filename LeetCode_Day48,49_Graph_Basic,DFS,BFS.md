# LeetCode Day 48,49 Graph

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

```python
def dfs(graph, x, n, path, result):
    if x == n:
        result.append(path.copy())
        return
    for i in range(1, n + 1):
        if graph[x][i] == 1:
            path.append(i)
            dfs(graph, i, n, path, result)
            path.pop()

def main():
    n, m = map(int, input().split())
    graph = [[0] * (n + 1) for _ in range(n + 1)]

    for _ in range(m):
        s, t = map(int, input().split())
        graph[s][t] = 1

    result = []
    dfs(graph, 1, n, [1], result)

    if not result:
        print(-1)
    else:
        for path in result:
            print(' '.join(map(str, path)))

if __name__ == "__main__":
    main()
```



### [797. All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

```python
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:

        target = len(graph) - 1
        results = []

        def backtrack(curr_node, path):
            # if we reach the target, no need to explore further.
            if curr_node == target:
                results.append(list(path))
                return
            # explore the neighbor nodes one after another.
            for next_node in graph[curr_node]:
                path.append(next_node)
                backtrack(next_node, path)
                path.pop()
                
        # kick of the backtracking, starting from the source node (0).
        path = [0]
        backtrack(0, path)

        return results
```

