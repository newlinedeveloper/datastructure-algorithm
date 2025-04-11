Absolutely! Let's break down **BFS Variants** — especially **0-1 BFS** and **Multi-source BFS**, which are often used when you're dealing with special edge weights or multiple starting points.

---

## 💡 Pattern 10: BFS Variants (0-1 BFS, Multi-source BFS)

---

### 🔍 How to Identify This Pattern:

#### ✅ 0-1 BFS
- Graph edges have only **0 or 1 weight**
- You’re finding **shortest path**
- You want Dijkstra’s efficiency without priority queue overhead

#### ✅ Multi-source BFS
- Problem mentions **multiple starting points**
- Spread-based processes: infection, fire, signal, etc.
- You want to **minimize time/distance** to spread to all reachable cells

---

### 🧠 Key Techniques:

| Variant         | Technique                        | Data Structure |
|----------------|-----------------------------------|----------------|
| 0-1 BFS         | Use deque (`pushFront` for 0s)    | `deque`        |
| Multi-source BFS| Add all sources initially in BFS | `queue`        |

---

## 🧪 Problem 1: **Rotting Oranges**  
📌 LeetCode 994  
👀 Multi-source BFS

---

### ✅ Problem Summary:
Each rotten orange spreads to adjacent fresh oranges per minute. Return the time until all oranges rot.

---

### ✅ Go Code:

```go
func orangesRotting(grid [][]int) int {
    rows, cols := len(grid), len(grid[0])
    queue := [][]int{}
    fresh := 0

    // Initial state
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if grid[r][c] == 2 {
                queue = append(queue, []int{r, c})
            } else if grid[r][c] == 1 {
                fresh++
            }
        }
    }

    if fresh == 0 {
        return 0
    }

    time := 0
    dirs := [][2]int{{0,1},{1,0},{0,-1},{-1,0}}

    for len(queue) > 0 {
        next := [][]int{}
        for _, pos := range queue {
            r, c := pos[0], pos[1]
            for _, d := range dirs {
                nr, nc := r+d[0], c+d[1]
                if nr >= 0 && nr < rows && nc >= 0 && nc < cols && grid[nr][nc] == 1 {
                    grid[nr][nc] = 2
                    fresh--
                    next = append(next, []int{nr, nc})
                }
            }
        }
        if len(next) > 0 {
            time++
        }
        queue = next
    }

    if fresh > 0 {
        return -1
    }
    return time
}
```

### ✅ Input:
```go
grid := [][]int{
    {2, 1, 1},
    {1, 1, 0},
    {0, 1, 1},
}
```

### ✅ Output:
```
4
```

### 💡 Explanation:
All rotten oranges start together — **multi-source BFS** spreads level by level.

---

## 🧪 Problem 2: **01 Matrix**  
📌 LeetCode 542  
👀 Multi-source BFS

---

### ✅ Problem Summary:
Given a binary matrix, return the distance of each cell to the nearest 0.

---

### ✅ Go Code:

```go
func updateMatrix(mat [][]int) [][]int {
    rows, cols := len(mat), len(mat[0])
    dist := make([][]int, rows)
    queue := [][]int{}

    for i := 0; i < rows; i++ {
        dist[i] = make([]int, cols)
        for j := 0; j < cols; j++ {
            if mat[i][j] == 0 {
                dist[i][j] = 0
                queue = append(queue, []int{i, j})
            } else {
                dist[i][j] = -1 // unvisited
            }
        }
    }

    dirs := [][2]int{{0,1},{1,0},{0,-1},{-1,0}}

    for len(queue) > 0 {
        cell := queue[0]
        queue = queue[1:]
        for _, d := range dirs {
            nr, nc := cell[0]+d[0], cell[1]+d[1]
            if nr >= 0 && nr < rows && nc >= 0 && nc < cols && dist[nr][nc] == -1 {
                dist[nr][nc] = dist[cell[0]][cell[1]] + 1
                queue = append(queue, []int{nr, nc})
            }
        }
    }

    return dist
}
```

### ✅ Input:
```go
mat := [][]int{
    {0,0,0},
    {0,1,0},
    {1,1,1},
}
```

### ✅ Output:
```
[[0 0 0]
 [0 1 0]
 [1 2 1]]
```

### 💡 Explanation:
All 0s are BFS sources — distances grow outward level by level.

---

## 🧪 Problem 3: **Shortest Path with Obstacles Elimination**  
📌 LeetCode 1293  
👀 0-1 BFS (could also use Dijkstra)

---

### ✅ Problem Summary:
You can eliminate up to `k` obstacles in a grid. Find the shortest path from top-left to bottom-right.

---

### ✅ Go Code:

```go
type State struct {
    r, c, steps, k int
}

func shortestPath(grid [][]int, k int) int {
    rows, cols := len(grid), len(grid[0])
    visited := make([][][]bool, rows)
    for i := range visited {
        visited[i] = make([][]bool, cols)
        for j := range visited[i] {
            visited[i][j] = make([]bool, k+1)
        }
    }

    queue := []State{{0, 0, 0, k}}
    dirs := [][2]int{{0,1},{1,0},{0,-1},{-1,0}}

    for len(queue) > 0 {
        curr := queue[0]
        queue = queue[1:]

        if curr.r == rows-1 && curr.c == cols-1 {
            return curr.steps
        }

        for _, d := range dirs {
            nr, nc := curr.r+d[0], curr.c+d[1]
            if nr >= 0 && nr < rows && nc >= 0 && nc < cols {
                nk := curr.k - grid[nr][nc]
                if nk >= 0 && !visited[nr][nc][nk] {
                    visited[nr][nc][nk] = true
                    queue = append(queue, State{nr, nc, curr.steps + 1, nk})
                }
            }
        }
    }

    return -1
}
```

### ✅ Input:
```go
grid := [][]int{
    {0,1,1},
    {1,1,1},
    {1,0,0},
}
k := 1
```

### ✅ Output:
```
6
```

### 💡 Explanation:
0-1 BFS based on whether you use your obstacle removal (1-cost move) or not (0-cost move).

---

## ✅ Summary:

| Problem                           | Pattern Type    | BFS Variant       | Key Idea                           |
|----------------------------------|------------------|--------------------|-------------------------------------|
| Rotting Oranges                  | Spread from all sources | Multi-source BFS | Spread simultaneously from all rotten |
| 01 Matrix                        | Nearest 0 distance       | Multi-source BFS | Use all 0s as starting points       |
| Shortest Path w/ Obstacles      | Min path + eliminations | 0-1 BFS / BFS + state | Encode (position + obstacles left) |

---
