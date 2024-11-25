# LeetCode Day 56 Dijkstra



[Shortest Path Algorithm Tutorial with Problems](https://www.geeksforgeeks.org/shortest-path-algorithms-a-complete-guide/)

### Single Source Shortest Path Algorithms:

In this algorithm we determine the shortest path of all the nodes in the graph with respect to a single node i.e. we have only one source node in this algorithms.

1. [Depth-First Search (DFS)](https://www.geeksforgeeks.org/depth-first-search-or-dfs-for-a-graph/)
2. [Breadth-First Search (BFS)](https://www.geeksforgeeks.org/breadth-first-search-or-bfs-for-a-graph/)
3. [Multi-Source BFS](https://www.geeksforgeeks.org/multi-source-shortest-path-in-unweighted-graph/)
4. [Dijkstra’s algorithm](https://www.geeksforgeeks.org/dijkstras-shortest-path-algorithm-greedy-algo-7/)
5. [Bellman-Ford algorithm](https://www.geeksforgeeks.org/bellman-ford-algorithm-dp-23/)
6. [Topological Sort](https://www.geeksforgeeks.org/topological-sorting/)
7. [A* search algorithm](https://www.geeksforgeeks.org/a-search-algorithm/)

### All Pair Shortest Path Algorithms:

Contrary to the single source shortest path algorithm, in this algorithm we determine the shortest path between every possible pair of nodes.

1. [Floyd-Warshall algorithm](https://www.geeksforgeeks.org/floyd-warshall-algorithm-dp-16/)
2. [Johnson’s algorithm](https://www.geeksforgeeks.org/johnsons-algorithm/)


### Dijkstra

Dijkstra algorithm: An algorithm for finding the shortest path from a starting point to other nodes in a weighted graph (weights are non-negative).

Steps:
1. Selet an unvisited node which is closest to the source node. 
2. Mark the node as visited
3. Update the distance from remianing unvisited nodes to the source node (ie. update the `minDist` array)

The steps are similar with Prim's Algorithm, but `minDist` in Prim's is the distance between the node and the MST. While in Dijstra, is the distance between the node and the start node.

[47. 参加科学大会（第六期模拟笔试）](https://kamacoder.com/problempage.php?pid=1047)
```python
n, m = map(int, input().split())

edges = []
graph = [[float('inf')]*(n+1) for _ in range(n+1)]
for _ in range(m):
    s, t, w = map(int, input().split())
    edges.append([s,t,w])
    graph[s][t] = w
    
minDist = [float('inf')] * (n+1)
visited = [False] * (n+1)

minDist[1] = 0

for i in range(1, n+1):
    # Step1: Selet an unvisited node which is closest to the source node. 
    minVal = float('inf')
    cur = -1
    
    for j in range(1, n+1):
        if not visited[j] and minDist[j] < minVal:
            minVal = minDist[j]
            cur = j
    
    if cur == -1: break

    # Step2: Mark the node as visited
    visited[cur] = True
    
    # Step3: Update the distance from remianing unvisited nodes to the source node.
    for j in range(1, n+1):
        if not visited[j] and graph[cur][j]!=float('inf'):
            if minDist[cur] + graph[cur][j] < minDist[j]:
                minDist[j] = minDist[cur] + graph[cur][j]

if minDist[n] != float('inf'):
    print(minDist[n])
else:
    print(-1)
```

**Time Complexity**: O(v^2)

#### Optimize using heap:

**Time Complexity**: O(E * (V + logE))


