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







### [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)

#### Approach1: Using DFS

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        direction = [[0, 1], [1, 0], [0, -1], [-1, 0]] 
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for _ in range(m)]


        def dfs(grid, visited, x, y):
            for i, j in direction:
                next_x = x + i
                next_y = y + j
            
                if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
                    continue
                # 未访问的陆地，标记并调用深度优先搜索
                if not visited[next_x][next_y] and grid[next_x][next_y] == "1":
                    visited[next_x][next_y] = True
                    dfs(grid, visited, next_x, next_y)

        res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == "1" and not visited[i][j]:
                    res += 1
                    visited[i][j] = 1
                    dfs(grid, visited, i, j)

        return res
```



Improvement: If find a "1", count, then using DFS to change all nearby "1" to "0". 

But for this way, we changed the original `grid`, while the previous approach keep the `grid` unchanged.

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        direction = [[0, 1], [1, 0], [0, -1], [-1, 0]] 
        m = len(grid)
        n = len(grid[0])
        res = 0

        def dfs(x, y):
            if not 0 <= x < m or not 0 <= y < n or grid[x][y] == "0":
                return
            
            grid[x][y] = "0"
            for i, j in direction:
                dfs(x+i, y+j)
            
        for x in range(m):
            for y in range(n):
                if grid[x][y] == '1':
                    dfs(x, y)
                    res += 1
        return res
```



#### Approach 2: BFS

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        direction = [[0, 1], [1, 0], [0, -1], [-1, 0]] 
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for _ in range(m)]
        res = 0

        def bfs(x,y):
            que = deque([])
            que.append([x,y])

            while que:
                cur_x, cur_y = que.popleft()
                for i, j in direction:
                    if 0<=cur_x+i<len(grid) and 0<=cur_y+j<len(grid[0]):
                        if not visited[cur_x+i][cur_y+j] and grid[cur_x+i][cur_y+j]=="1":
                            visited[cur_x+i][cur_y+j] = 1
                            que.append([cur_x+i, cur_y+j])
        
        for x in range(m):
            for y in range(n):
                if grid[x][y] == "1" and not visited[x][y]:
                    visited[x][y] = 1
                    res += 1
                    bfs(x,y)
        return res
```



Same, we improve simplicity by changing all "1" to "0":

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        direction = [[0, 1], [1, 0], [0, -1], [-1, 0]] 
        m = len(grid)
        n = len(grid[0])

        def bfs(x,y):
            que = deque([])
            que.append([x,y])

            while que:
                cur_x, cur_y = que.popleft()

                for i, j in direction:
                    next_x = cur_x + i
                    next_y = cur_y + j
                    
                    if 0<=next_x<m and 0<=next_y<n:
                        if grid[next_x][next_y] == "1":
                            grid[next_x][next_y] = "0"
                            que.append([next_x, next_y])
        
        res = 0
        for x in range(m):
            for y in range(n):
                if grid[x][y] == "1":
                    res += 1
                    bfs(x,y)

        return res
```



#### [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/)

Using the same template as last question, just adding a area calculating logic.

BFS:

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        
        direction = [[0, 1], [1, 0], [0, -1], [-1, 0]] 
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for _ in range(m)]
        res = 0

        def bfs(x,y):
            que = deque([])
            que.append([x,y])
            area = 0

            while que:
                cur_x, cur_y = que.popleft()
                print(cur_x, cur_y)
                area += 1
                for i, j in direction:
                    if 0<=cur_x+i<len(grid) and 0<=cur_y+j<len(grid[0]):
                        if not visited[cur_x+i][cur_y+j] and grid[cur_x+i][cur_y+j]==1:
                            visited[cur_x+i][cur_y+j] = 1
                            que.append([cur_x+i, cur_y+j])
            return area
        
        res = 0
        for x in range(m):
            for y in range(n):
                if grid[x][y] == 1 and not visited[x][y]:
                    visited[x][y] = 1
                    res = max(res, bfs(x,y))
                    print("res:", res)
        return res
```

