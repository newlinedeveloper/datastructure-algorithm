Let’s explore the **Backtracking in Graphs** pattern — essential when dealing with exhaustive search, puzzles, and multi-path exploration problems.

---

## 💡 Pattern 9: Backtracking in Graphs

### 🔍 How to Identify This Pattern:
Look for keywords like:
- “Find all possible paths”, “can you reach?”
- "Return all valid configurations"
- “Explore all ways to...”
- Graphs or grids with **choices at each step**

Often combined with:
- DFS (Depth-First Search)
- Recursion + visited tracking
- **Undoing** (backtracking) the last move after recursion

---

## 🧠 Core Technique:
- Use **DFS** to explore paths
- Mark nodes/positions as **visited**
- After recursion, **unmark** (backtrack) to explore other paths

---

## 🧪 Problem 1: **Word Search**  
📌 LeetCode 79

### ✅ Problem Summary:
Given a 2D board and a word, check if the word exists in the board using adjacent cells (no reuse).

---

### ✅ Go Code:

```go
func exist(board [][]byte, word string) bool {
    rows, cols := len(board), len(board[0])
    visited := make([][]bool, rows)
    for i := range visited {
        visited[i] = make([]bool, cols)
    }

    var dfs func(r, c, i int) bool
    dfs = func(r, c, i int) bool {
        if i == len(word) {
            return true
        }
        if r < 0 || c < 0 || r >= rows || c >= cols || visited[r][c] || board[r][c] != word[i] {
            return false
        }

        visited[r][c] = true
        defer func() { visited[r][c] = false }() // backtrack

        dirs := [][2]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
        for _, d := range dirs {
            if dfs(r+d[0], c+d[1], i+1) {
                return true
            }
        }

        return false
    }

    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if dfs(r, c, 0) {
                return true
            }
        }
    }
    return false
}
```

---

### ✅ Input:
```go
board := [][]byte{
    {'A', 'B', 'C', 'E'},
    {'S', 'F', 'C', 'S'},
    {'A', 'D', 'E', 'E'},
}
word := "ABCCED"
```

### ✅ Output:
```
true
```

### 💡 Explanation:
Starts from every cell, recursively matches each letter, backtracks when needed.

---

## 🧪 Problem 2: **All Paths From Source to Target**  
📌 LeetCode 797

### ✅ Problem Summary:
Given a DAG (directed acyclic graph), return **all possible paths** from node 0 to node n-1.

---

### ✅ Go Code:

```go
func allPathsSourceTarget(graph [][]int) [][]int {
    var res [][]int
    var path []int

    var dfs func(int)
    dfs = func(node int) {
        path = append(path, node)
        if node == len(graph)-1 {
            res = append(res, append([]int{}, path...))
        } else {
            for _, neighbor := range graph[node] {
                dfs(neighbor)
            }
        }
        path = path[:len(path)-1] // backtrack
    }

    dfs(0)
    return res
}
```

---

### ✅ Input:
```go
graph := [][]int{{1, 2}, {3}, {3}, {}}
```

### ✅ Output:
```
[[0,1,3],[0,2,3]]
```

### 💡 Explanation:
DFS explores all paths, backtracking allows reuse of nodes in different paths.

---

## 🧪 Problem 3: **The Maze**  
📌 LeetCode 490

### ✅ Problem Summary:
Given a maze with walls and open spaces, determine if a ball can **stop** at the destination. The ball rolls until it hits a wall.

---

### ✅ Go Code:

```go
func hasPath(maze [][]int, start []int, destination []int) bool {
    rows, cols := len(maze), len(maze[0])
    visited := make([][]bool, rows)
    for i := range visited {
        visited[i] = make([]bool, cols)
    }

    dirs := [][2]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

    var dfs func(x, y int) bool
    dfs = func(x, y int) bool {
        if visited[x][y] {
            return false
        }
        if x == destination[0] && y == destination[1] {
            return true
        }

        visited[x][y] = true
        for _, d := range dirs {
            nx, ny := x, y
            // keep rolling
            for nx+d[0] >= 0 && nx+d[0] < rows && ny+d[1] >= 0 && ny+d[1] < cols && maze[nx+d[0]][ny+d[1]] == 0 {
                nx += d[0]
                ny += d[1]
            }
            if dfs(nx, ny) {
                return true
            }
        }
        return false
    }

    return dfs(start[0], start[1])
}
```

---

### ✅ Input:
```go
maze := [][]int{
    {0, 0, 1, 0, 0},
    {0, 0, 0, 0, 0},
    {0, 0, 0, 1, 0},
    {1, 1, 0, 1, 1},
    {0, 0, 0, 0, 0},
}
start := []int{0, 4}
destination := []int{4, 4}
```

### ✅ Output:
```
true
```

---

## ✅ Summary:

| Problem                      | Identify By                                        | Backtracking Role                   |
|------------------------------|----------------------------------------------------|-------------------------------------|
| Word Search                  | “Can this word be formed?” in grid                 | Explore all directions recursively  |
| All Paths From Source to Target | “List all paths” in DAG                        | Track path, backtrack for next path |
| The Maze                     | “Can ball reach destination?” while rolling        | DFS with custom stop condition      |

---
