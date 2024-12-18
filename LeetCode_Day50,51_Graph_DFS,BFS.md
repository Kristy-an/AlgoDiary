# LeetCode Day 50,51 Graph



### Catalog:

Changing "island" to "water" or vice

- [Karmar101.The total area of isolated islands](#problem1)

- [Kamar102.Sink isolated islands](#problem2)

Great tricks to optimize from brute=-force

- [417. Pacific Atlantic Water Flow](#problem3)

- [827. Making A Large Island](#problem4)

Applying graph

- [127. Word Ladder](#problem5)

Other

- [463. Island Perimeter](#problem5)



### [卡码网：101. 孤岛的总面积](https://kamacoder.com/problempage.php?pid=1173)  The total area of the island<a name="problem1"></a>

**Problem Summary:** Calculating the area of all islands that not touch the edges of the matrixs.

**Solution:** Usnig bfs/dfs to search all islands on the edges of the matrixsand, and change these gound (1) to water (0). Then solve the sum of remaining matrix, we will get the area of isolated islands.

#### **Approach: bfs**

```python
from collections import deque


def bfs(x, y):
    que = deque([])
    que.append([x,y])
    grid[x][y] = 0
    
    while que:
        x, y = que.popleft()
        
        for i, j in directions:
            next_x = x + i
            next_y = y + j
            if next_x<0 or next_x>=m or next_y<0 or next_y>=n:
                continue
            
            if grid[next_x][next_y] == 1:
                grid[next_x][next_y] = 0
                que.append([next_x, next_y])
    
    

if __name__ == "__main__":
    m, n = list(map(int, input().split()))
    grid = []
    for _ in range(m):
       row = list(map(int, input().split()))
       grid.append(row)

    directions  = [[0,1], [0,-1], [1,0], [-1,0]]
    count = 0

    for x in range(m):
        if grid[x][0] == 1: bfs(x, 0)
        if grid[x][n-1] == 1: bfs(x, n-1)
    for y in range(n):
        if grid[0][y] == 1: bfs(0, y)
        if grid[m-1][y] == 1: bfs(m-1, y)
    
    area = 0
    
    for row in grid:
        area += sum(row)
    print(area)
```



### [102. 沉没孤岛](https://kamacoder.com/problempage.php?pid=1174)<a name="problem2"></a>

Paird with Problem 101 and opposite to it. In 101 change all islands on edges to water, in 102, we change all isolated islands to water. 

We can also first search islands on edges, lable them (like change 1 to 2). Then change all remaining island to water. Finally remove the labels (change 2 back to 1).



### [103. 水流问题](https://kamacoder.com/problempage.php?pid=1175) / [417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow) <a name="problem3"></a>

With **broute-force**, we iterate through each point of the grid, using dfs/bfs to lable all the accessible points, then determine whether these points reach two borders. However, the time complexity will be O(m<sup>2</sup>n<sup>2</sup>).

To **optimize** the time complexity, we can think about it the other way around, going upstream from the nodes on the first set of boundaries and marking all the nodes that have been traversed.

Similarly, going upstream from the edge nodes on the second set of boundaries and marking the nodes that have been traversed.

Then the nodes marked by both sides are the nodes that can flow into both the Pacific Ocean and the Atlantic Ocean.

#### Approach: DFS

```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        directions  = [[0,1], [0,-1], [1,0], [-1,0]]
        m = len(heights)
        n = len(heights[0])

        def dfs(x, y, visited):
            visited[x][y] = 1

            for i, j in directions:
                next_x = x + i
                next_y = y + j

                if next_x < 0 or next_y < 0 or next_x >= m or next_y >= n: continue
                if not visited[next_x][next_y] and heights[x][y] <= heights[next_x][next_y]: 
                    dfs(next_x, next_y, visited)
            return
        
        visited1 = [[0]*n for _ in range(m)]
        visited2 = [[0]*n for _ in range(m)]
        
        for x in range(m):
            dfs(x, 0, visited1)
            dfs(x, n-1, visited2)
        for y in range(n):
            dfs(0, y, visited1)
            dfs(m-1, y, visited2)
        
        res = []
        for x in range(m):
            for y in range(n):
                if visited1[x][y] == 1 and visited2[x][y] == 1:
                    res.append([x,y])
        return res
```



### [827. Making A Large Island](https://leetcode.com/problems/making-a-large-island/) /[104. 建造最大岛屿](https://kamacoder.com/problempage.php?pid=1176) <a name="problem4"></a>





### [127. Word Ladder](https://leetcode.com/problems/word-ladder/) <a name="problem5"></a>



### [463. Island Perimeter](https://leetcode.com/problems/island-perimeter/) <a name="problem6"></a>

```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        res = 0
        for i in range(len(grid)):
            for j in range(len(grid[i])):
                if grid[i][j] == 1:
                    if i == 0 or grid[i-1][j]==0:
                        res += 1
                    if i == len(grid) - 1 or grid[i+1][j]==0:
                        res += 1
                    if j == 0 or grid[i][j-1] == 0:
                        res += 1
                    if j == len(grid[i]) - 1 or grid[i][j+1]==0:
                        res += 1
        return res
```

