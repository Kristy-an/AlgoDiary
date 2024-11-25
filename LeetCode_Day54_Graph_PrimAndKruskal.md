
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

Prim's algorithm maintains a set of nodes, while Kruskal's maintains a set of edges.

## Prim’s Algorithm

1. 第一步，选距离生成树最近节点
2. 第二步，最近节点加入生成树
3. 第三步，更新非生成树节点到生成树的距离（即更新minDist数组）

[53. 寻宝（第七期模拟笔试）](https://kamacoder.com/problempage.php?pid=1053)
```python
v, e = map(int, input().split())

graph = [[10001]*(v+1) for _ in range(v+1)]

for _ in range(e):
    m, n, weight = map(int, input().split())
    graph[m][n] = weight
    graph[n][m] = weight
    

visited = [0]*(v+1)
minDist = [10001]*(v+1)
minDist[0] = 0

# Add a start node to MST
visited[1] = 1
minDist[1] = 0
# Update the distance of other nodes
for i in range(v+1):
    minDist[i] = min(minDist[i], graph[1][i], graph[i][1])


while sum(visited) < v:
    minidx = 0
    mindis = 10001
    for i in range(1, v+1):
        if minDist[i] < mindis and visited[i]==0:
            mindis = minDist[i]
            minidx = i
    # Add the nearest node to MST
    visited[minidx] = 1
    # Update the distance of other nodes
    for i in range(1, v+1):
        if visited[i] == 0 and minDist[i] > graph[minidx][i]:
            minDist[i] = graph[minidx][i]

print(sum(minDist))
```


## Kruskal’s Algorithm
Steps:
-   边的权值排序，因为要优先选最小的边加入到生成树里
-   遍历排序后的边
    -   如果边首尾的两个节点在同一个集合，说明如果连上这条边图中会出现环
    -   如果边首尾的两个节点不在同一个集合，加入到最小生成树，并把两个节点加入同一个集合

Therefore, we will need the UnionFind data structure to determine whether two nodes are in a same set. 

```python
class Edge:
    def __init__(self, l, r, val):
        self.l = l
        self.r = r
        self.val = val

n = 10001
father = list(range(n))

def init():
    global father
    father = list(range(n))

def find(u):
    if u != father[u]:
        father[u] = find(father[u])
    return father[u]

def join(u, v):
    u = find(u)
    v = find(v)
    if u != v:
        father[v] = u

def kruskal(v, edges):
    edges.sort(key=lambda edge: edge.val)
    init()
    result_val = 0

    for edge in edges:
        x = find(edge.l)
        y = find(edge.r)
        if x != y:
            result_val += edge.val
            join(x, y)

    return result_val

if __name__ == "__main__":
    import sys
    input = sys.stdin.read
    data = input().split()

    v = int(data[0])
    e = int(data[1])

    edges = []
    index = 2
    for _ in range(e):
        v1 = int(data[index])
        v2 = int(data[index + 1])
        val = int(data[index + 2])
        edges.append(Edge(v1, v2, val))
        index += 3

    result_val = kruskal(v, edges)
    print(result_val)
 ```
