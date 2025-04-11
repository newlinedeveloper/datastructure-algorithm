Here's a detailed breakdown of the **Matrix DP Pattern**, including how to identify such problems, with **Golang solutions** and **sample inputs/outputs** for each.

---

## 🧩 6. Matrix DP Pattern — Navigate through a grid using optimal path logic

---

### 🔍 **How to Identify This Pattern**

| Clue in Problem Statement | Hints at |
|---------------------------|----------|
| "Find the minimum/maximum path cost from top-left to bottom-right" | Grid traversal with optimization |
| "Move only down or right" | DP over 2D matrix |
| "With obstacles or blocked paths" | Conditional transitions |
| "Form square or rectangle" | Submatrix or region problems |
| "Collect max cherries or rewards on two passes" | Multiple-pass matrix traversal |

Common DP table layout: `dp[i][j]` stores optimal value (min/max/count) to reach cell `(i,j)`.

---

## 🟢 1. Minimum Path Sum — [LeetCode #64](https://leetcode.com/problems/minimum-path-sum)

### ✅ Problem

> Find a path from top-left to bottom-right of a grid which minimizes the sum of all numbers along its path. You can only move right or down.

---

### ✅ Golang Code:

```go
func minPathSum(grid [][]int) int {
    m, n := len(grid), len(grid[0])
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }

    dp[0][0] = grid[0][0]

    // Fill first row and column
    for i := 1; i < m; i++ {
        dp[i][0] = dp[i-1][0] + grid[i][0]
    }
    for j := 1; j < n; j++ {
        dp[0][j] = dp[0][j-1] + grid[0][j]
    }

    // Fill the rest
    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
        }
    }

    return dp[m-1][n-1]
}

func min(a, b int) int {
    if a < b { return a }
    return b
}
```

### 🧪 Sample I/O:

**Input:**
```go
grid = [
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
```

**Output:** `7`  
**Explanation:** Path: `1 → 3 → 1 → 1 → 1`

---

## 🟢 2. Unique Paths — [LeetCode #62](https://leetcode.com/problems/unique-paths)

### ✅ Problem

> Count how many unique paths exist from top-left to bottom-right in an `m x n` grid, moving only down or right.

---

### ✅ Golang Code:

```go
func uniquePaths(m int, n int) int {
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }

    for i := 0; i < m; i++ {
        dp[i][0] = 1
    }
    for j := 0; j < n; j++ {
        dp[0][j] = 1
    }

    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
        }
    }

    return dp[m-1][n-1]
}
```

### 🧪 Sample I/O:

**Input:** `m = 3, n = 7`  
**Output:** `28`

---

## 🟢 3. Unique Paths II (with Obstacles) — [LeetCode #63](https://leetcode.com/problems/unique-paths-ii)

### ✅ Problem

> Like Unique Paths, but some cells have obstacles (`1`), and paths cannot go through them.

---

### ✅ Golang Code:

```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {
    m, n := len(obstacleGrid), len(obstacleGrid[0])
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }

    // Starting point
    if obstacleGrid[0][0] == 0 {
        dp[0][0] = 1
    }

    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if obstacleGrid[i][j] == 1 {
                dp[i][j] = 0
            } else {
                if i > 0 {
                    dp[i][j] += dp[i-1][j]
                }
                if j > 0 {
                    dp[i][j] += dp[i][j-1]
                }
            }
        }
    }

    return dp[m-1][n-1]
}
```

### 🧪 Sample I/O:

**Input:**
```go
[
 [0,0,0],
 [0,1,0],
 [0,0,0]
]
```

**Output:** `2`

---

## 🟢 4. Maximal Square — [LeetCode #221](https://leetcode.com/problems/maximal-square)

### ✅ Problem

> Given a binary matrix, find the largest square containing only `1`s and return its area.

---

### ✅ Golang Code:

```go
func maximalSquare(matrix [][]byte) int {
    if len(matrix) == 0 {
        return 0
    }

    rows, cols := len(matrix), len(matrix[0])
    dp := make([][]int, rows+1)
    for i := range dp {
        dp[i] = make([]int, cols+1)
    }

    maxSide := 0

    for i := 1; i <= rows; i++ {
        for j := 1; j <= cols; j++ {
            if matrix[i-1][j-1] == '1' {
                dp[i][j] = min(dp[i-1][j], min(dp[i][j-1], dp[i-1][j-1])) + 1
                maxSide = max(maxSide, dp[i][j])
            }
        }
    }

    return maxSide * maxSide
}

func max(a, b int) int {
    if a > b { return a }
    return b
}
```

### 🧪 Sample I/O:

**Input:**
```go
[
 ["1","0","1","0","0"],
 ["1","0","1","1","1"],
 ["1","1","1","1","1"],
 ["1","0","0","1","0"]
]
```

**Output:** `4` (2x2 square of 1s)

---

## 🔴 5. Cherry Pickup — [LeetCode #741](https://leetcode.com/problems/cherry-pickup)

### ✅ Problem

> Pick up the most cherries with 2 trips: top-left to bottom-right and back, avoiding -1 (thorns). Tricky 3D DP or memoization.

---

### ✅ Golang Code (Optimized with Memoization):

```go
func cherryPickup(grid [][]int) int {
    n := len(grid)
    memo := make(map[[3]int]int)

    var dp func(r1, c1, c2 int) int
    dp = func(r1, c1, c2 int) int {
        r2 := r1 + c1 - c2 // deduce r2 from (r1 + c1 = r2 + c2)
        if r1 >= n || r2 >= n || c1 >= n || c2 >= n ||
            grid[r1][c1] == -1 || grid[r2][c2] == -1 {
            return -1 << 31 // very small
        }

        if r1 == n-1 && c1 == n-1 {
            return grid[r1][c1]
        }

        key := [3]int{r1, c1, c2}
        if val, ok := memo[key]; ok {
            return val
        }

        cherries := grid[r1][c1]
        if c1 != c2 || r1 != r2 {
            cherries += grid[r2][c2]
        }

        maxCherries := max(
            dp(r1+1, c1, c2),
            dp(r1, c1+1, c2),
            dp(r1+1, c1, c2+1),
            dp(r1, c1+1, c2+1),
        )

        if maxCherries == -1<<31 {
            memo[key] = -1 << 31
            return memo[key]
        }

        memo[key] = cherries + maxCherries
        return memo[key]
    }

    result := dp(0, 0, 0)
    if result < 0 {
        return 0
    }
    return result
}
```

### 🧪 Sample I/O:

**Input:**
```go
[
 [0,1,-1],
 [1,0,-1],
 [1,1, 1]
]
```

**Output:** `5`

---

## 🧠 Summary Table

| Problem | Core Idea | Transition |
|--------|-----------|------------|
| Minimum Path Sum | Min cost | `dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]` |
| Unique Paths | Count paths | `dp[i][j] = dp[i-1][j] + dp[i][j-1]` |
| Unique Paths II | Count paths with blocks | Same as above but skip obstacles |
| Maximal Square | Largest square | `dp[i][j] = min(top, left, diag) + 1` |
| Cherry Pickup | 2-pass, collect max | 3D DP with memoization |

---
