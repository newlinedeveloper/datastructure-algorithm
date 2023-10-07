### Graph programming

1. No of island problem

```

from collections import deque

def noOfIslands(grid: list[list[str]]):
    res = 0
    Row , Col = len(grid), len(grid[0])
    islandsCount = 0
    visited = set()
    
    def bfs(row:str, col:str):
        queue = deque()
        queue.append((row, col))
        visited.add((row, col))
        
        while queue:
            dr, dc = queue.popleft()
            directions = [[1,0],[-1,0],[0,1],[0,-1]]
            for r, c in directions:
                dRow, dCol = dr+r, dc+c
                # print(grid[dRow][dCol])
                
                if dRow >=0 and dRow < Row and dCol >= 0 and dCol < Col and grid[dRow][dCol] == "1" and (dRow, dCol) not in visited:
                    # print(grid[dRow][dCol])
                    queue.append((dRow, dCol))
                    visited.add((dRow,dCol))
        
        # print(row, col)
    
    for i in range(Row):
        for j in range(Col):
            if grid[i][j] == "1" and (i,j) not in visited:
                bfs(i, j)
                islandsCount += 1
                
    return islandsCount
    


grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]

print(noOfIslands(grid))

```
- 2. Clone Graph
 
```
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

from typing import Optional
class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        oldToNew = {}

        def dfs(node):
            if node in oldToNew:
                return oldToNew[node]
            
            copy = Node(node.val)
            oldToNew[node] = copy
            for nei in node.neighbors:
                copy.neighbors.append(dfs(nei))
            return copy
        
        return dfs(node) if node else None

```

3. Max Area of Island

```
res = 0
    Row , Col = len(grid), len(grid[0])
    islandsCount = 0
    visited = set()
    maxIslandArea = 0
    
    def bfs(row:str, col:str):
        queue = deque()
        queue.append((row, col))
        visited.add((row, col))
        islandArea = 1
        
        while queue:
            dr, dc = queue.popleft()
            directions = [[1,0],[-1,0],[0,1],[0,-1]]
            for r, c in directions:
                dRow, dCol = dr+r, dc+c
                # print(grid[dRow][dCol])
                
                if dRow >=0 and dRow < Row and dCol >= 0 and dCol < Col and grid[dRow][dCol] == 1 and (dRow, dCol) not in visited:
                    # print(grid[dRow][dCol])
                    queue.append((dRow, dCol))
                    visited.add((dRow,dCol))
                    islandArea += 1
        
        return islandArea
        # print(row, col)
    
    for i in range(Row):
        for j in range(Col):
            if grid[i][j] == 1 and (i,j) not in visited:
                area = bfs(i, j)
                islandsCount += 1
                maxIslandArea = max(maxIslandArea, area)
                
    return maxIslandArea

```


4- Pacific Atlantic Water Flow

```


def pacificAtlantic(heights):
    res = []
    pac, atl = set(), set()
    ROWS, COLS = len(heights), len(heights[0]) 
    
    def dfs(row, col, visit, prevHeight):
        if ((row, col) in visit or row < 0 or col < 0 or
            row == ROWS or
            col == COLS or
            heights[row][col] < prevHeight
            ):
                return
            
        visit.add((row, col))
        dfs(row+1, col, visit, heights[row][col])
        dfs(row-1, col, visit, heights[row][col])
        dfs(row, col+1, visit, heights[row][col])
        dfs(row, col-1, visit, heights[row][col])
                
        
    # // Calculating pacific ocean reaching cells
    for C in range(COLS):
        dfs(0,C, pac, heights[0][C])
        dfs(ROWS-1, C, atl, heights[ROWS-1][C])
    
    
    # //Calculating Atlantic ocean reaching cells
    for R in range(ROWS):
        dfs(R, 0, pac, heights[R][0])
        dfs(R, COLS-1, atl, heights[R][COLS-1])
    

    
    for i in range(ROWS):
        for j in range(COLS):
            if (i,j) in atl and (i,j) in pac:
                res.append([i,j])
    
    # for (r,c) in pac:
    #     if (r,c) in atl:
    #         res.append([r, c])
            
    return res
    
    
heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]

result = pacificAtlantic(heights)
print(result)
    
```

