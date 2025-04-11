Absolutely! Let's break down the **Traversal Pattern (DFS/BFS)** in Graph DSA with **problem identification**, **Go code**, **step-by-step explanation**, and **sample I/O** for each.

---

## 🔁 1. Traversal Pattern (DFS/BFS)

### 🔍 How to Identify This Pattern

You'll likely need DFS/BFS when:
- The problem involves **visiting all nodes**, **exploring regions**, or **counting connected components**.
- Keywords:  
  > "Number of regions", "Find area/size", "Explore all neighbors", "How many groups/ islands", "Steps to reach", "Shortest path in grid".
- You're given a **grid**, **graph**, or **matrix** and asked to perform some kind of exploration.

---

## 🧪 Problem 1: **Number of Islands**
> https://leetcode.com/problems/number-of-islands/

### 🧾 Problem Summary:
Given a 2D grid of `'1'` (land) and `'0'` (water), count the number of islands. An island is surrounded by water and formed by connecting adjacent lands **horizontally or vertically**.

---

### ✅ Sample Input:
```txt
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```

### ✅ Expected Output:
```txt
3
```

---

### 🚀 Go Code (DFS):
```go
func numIslands(grid [][]byte) int {
    if len(grid) == 0 {
        return 0
    }

    rows := len(grid)
    cols := len(grid[0])
    count := 0

    var dfs func(r, c int)
    dfs = func(r, c int) {
        if r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] == '0' {
            return
        }

        grid[r][c] = '0' // mark visited

        // Explore all 4 directions
        dfs(r+1, c)
        dfs(r-1, c)
        dfs(r, c+1)
        dfs(r, c-1)
    }

    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if grid[r][c] == '1' {
                count++
                dfs(r, c)
            }
        }
    }

    return count
}
```

---

### 🧠 Explanation:
- We loop through each cell.
- If we find `'1'` (land), we trigger a **DFS** to mark all connected lands `'0'` (visited).
- Every DFS call = **one island**.

---

## 🧪 Problem 2: **Clone Graph**
> https://leetcode.com/problems/clone-graph/

### 🧾 Problem Summary:
Clone a given undirected graph. Return the **deep copy** of the graph starting from a given node.

---

### ✅ Sample Input:
```
Input: Node with value 1
1 — 2
|    |
4 — 3
```

### ✅ Expected Output:
A deep copy graph with same structure.

---

### 🚀 Go Code (DFS with Map):
```go
type Node struct {
    Val int
    Neighbors []*Node
}

func cloneGraph(node *Node) *Node {
    if node == nil {
        return nil
    }

    visited := make(map[*Node]*Node)

    var dfs func(*Node) *Node
    dfs = func(n *Node) *Node {
        if cloned, found := visited[n]; found {
            return cloned
        }

        copy := &Node{Val: n.Val}
        visited[n] = copy

        for _, neighbor := range n.Neighbors {
            copy.Neighbors = append(copy.Neighbors, dfs(neighbor))
        }

        return copy
    }

    return dfs(node)
}
```

---

### 🧠 Explanation:
- We use a **visited map** to track already cloned nodes (to handle cycles).
- Each node is copied recursively using DFS.
- For each neighbor, we recursively clone them too.

---

## 🧪 Problem 3: **Max Area of Island**
> https://leetcode.com/problems/max-area-of-island/

### 🧾 Problem Summary:
Find the **maximum area** of an island (connected `'1'`s) in a 2D grid.

---

### ✅ Sample Input:
```txt
grid = [
  [0,0,1,0,0],
  [0,1,1,1,0],
  [0,0,1,0,0],
  [0,0,0,0,0]
]
```

### ✅ Expected Output:
```txt
5
```

---

### 🚀 Go Code (DFS with Area Count):
```go
func maxAreaOfIsland(grid [][]int) int {
    maxArea := 0
    rows, cols := len(grid), len(grid[0])

    var dfs func(r, c int) int
    dfs = func(r, c int) int {
        if r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] == 0 {
            return 0
        }

        grid[r][c] = 0
        area := 1
        area += dfs(r+1, c)
        area += dfs(r-1, c)
        area += dfs(r, c+1)
        area += dfs(r, c-1)
        return area
    }

    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if grid[r][c] == 1 {
                maxArea = max(maxArea, dfs(r, c))
            }
        }
    }

    return maxArea
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

---

### 🧠 Explanation:
- Traverse every cell.
- When a `1` is found, launch DFS to explore full island and return its size.
- Keep track of the **maximum area** found.

---
