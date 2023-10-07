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

