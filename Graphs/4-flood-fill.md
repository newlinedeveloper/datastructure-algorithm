Let’s break down the **Flood Fill / Region Expansion** pattern — very common in matrix/grid-based problems and regularly asked in interviews at Google and other FAANG companies.

---

## 🔁 4. Flood Fill / Region Expansion Pattern

### 🔍 **How to Identify This Pattern**

Look for problems involving:
- **Spreading or filling** an area based on **adjacent criteria**
- Terms like: _“fill all adjacent”, “spread to neighbors”, “replace region”, “propagate value”_
- Usually represented using a **2D grid**, and involves **changing values** or **finding enclosed areas**

---

### 🧠 **How to Solve**

- Use **DFS (recursive or stack)** or **BFS (queue)**.
- Track visited cells (can be with a separate matrix or by modifying the grid itself).
- Use **direction vectors** to navigate in 4 or 8 directions.

---

## 🔹 Problem 1: **Flood Fill**  
📌 [Leetcode #733](https://leetcode.com/problems/flood-fill/)

Change the color of all connected cells that are the same as the starting cell.

### ✅ Sample Input:
```go
image = [
  [1,1,1],
  [1,1,0],
  [1,0,1]
]
sr = 1, sc = 1, newColor = 2
```

### ✅ Output:
```go
[
  [2,2,2],
  [2,2,0],
  [2,0,1]
]
```

---

### 🚀 Go Code (DFS):

```go
func floodFill(image [][]int, sr int, sc int, newColor int) [][]int {
    rows, cols := len(image), len(image[0])
    startColor := image[sr][sc]
    if startColor == newColor {
        return image
    }

    var dfs func(r, c int)
    dfs = func(r, c int) {
        if r < 0 || r >= rows || c < 0 || c >= cols || image[r][c] != startColor {
            return
        }
        image[r][c] = newColor
        dfs(r+1, c)
        dfs(r-1, c)
        dfs(r, c+1)
        dfs(r, c-1)
    }

    dfs(sr, sc)
    return image
}
```

### 🧠 Explanation:
- Start DFS at the given cell.
- Change all connected same-color cells to the `newColor`.
- Avoid loops by only visiting the original color.

---

## 🔹 Problem 2: **Surrounded Regions**  
📌 [Leetcode #130](https://leetcode.com/problems/surrounded-regions/)

Capture all regions surrounded by 'X' by flipping 'O' → 'X'. Do **not** flip 'O's connected to the border.

### ✅ Sample Input:
```go
board = [
  ['X','X','X','X'],
  ['X','O','O','X'],
  ['X','X','O','X'],
  ['X','O','X','X']
]
```

### ✅ Output:
```go
[
  ['X','X','X','X'],
  ['X','X','X','X'],
  ['X','X','X','X'],
  ['X','O','X','X']
]
```

---

### 🚀 Go Code (DFS from borders):

```go
func solve(board [][]byte) {
    if len(board) == 0 || len(board[0]) == 0 {
        return
    }

    rows, cols := len(board), len(board[0])

    var dfs func(r, c int)
    dfs = func(r, c int) {
        if r < 0 || r >= rows || c < 0 || c >= cols || board[r][c] != 'O' {
            return
        }
        board[r][c] = 'S' // safe
        dfs(r+1, c)
        dfs(r-1, c)
        dfs(r, c+1)
        dfs(r, c-1)
    }

    // Step 1: Mark all border-connected 'O's as safe
    for r := 0; r < rows; r++ {
        dfs(r, 0)
        dfs(r, cols-1)
    }
    for c := 0; c < cols; c++ {
        dfs(0, c)
        dfs(rows-1, c)
    }

    // Step 2: Flip all 'O' to 'X', and 'S' back to 'O'
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if board[r][c] == 'O' {
                board[r][c] = 'X'
            } else if board[r][c] == 'S' {
                board[r][c] = 'O'
            }
        }
    }
}
```

### 🧠 Explanation:
- Perform DFS from border 'O's.
- Temporarily mark visited 'O's as 'S' (safe).
- Convert unmarked 'O' to 'X' (captured), and 'S' back to 'O'.

---

## 🔹 Problem 3: **Walls and Gates**  
📌 [Leetcode #286](https://leetcode.com/problems/walls-and-gates/)

Fill each empty room with the distance to its nearest gate.

Absolutely! Let's break down **Leetcode #286: Walls and Gates** step by step.

---

## 🧩 Problem Statement: Walls and Gates

You're given a 2D grid (a matrix) where:
- `INF` (e.g., `2147483647`) represents an **empty room**
- `-1` represents a **wall or obstacle**
- `0` represents a **gate**

**Goal**:  
Fill each empty room with the **distance to its nearest gate**.  
If a gate is **unreachable**, leave the room as `INF`.

---

### 📌 Example Input:

```txt
[
  [INF,  -1,   0,  INF],
  [INF, INF, INF,  -1],
  [INF,  -1, INF,  -1],
  [0,    -1, INF, INF]
]
```

### ✅ Expected Output:

```txt
[
  [3,  -1,  0,  1],
  [2,   2, 1, -1],
  [1,  -1, 2, -1],
  [0,  -1, 3,  4]
]
```

---

## 🔍 How to Identify the Pattern?

This problem is a **multi-source BFS** problem:

- You’re spreading values from **multiple gates at once** (each gate acts as a BFS source).
- You need to calculate the **minimum distance** from the nearest gate to each cell.

💡 This fits the **Flood Fill / Region Expansion** + **Multi-source BFS** pattern.

---

## 🧠 Solution Approach

### BFS from All Gates (0s):

1. **Initialize a queue** with all the gate coordinates `(i, j)` (cells that have `0`)
2. Perform **BFS** from all gates simultaneously
3. For each `empty room (INF)` you reach, **update it with `distance + 1`**
4. Avoid visiting walls `(-1)` or revisiting visited cells

---

## ✅ Go Code Implementation

```go
package main

import (
	"fmt"
)

const INF = 2147483647

func wallsAndGates(rooms [][]int) {
	if len(rooms) == 0 || len(rooms[0]) == 0 {
		return
	}

	rows, cols := len(rooms), len(rooms[0])
	queue := [][]int{}

	// Step 1: Push all gates into the queue
	for r := 0; r < rows; r++ {
		for c := 0; c < cols; c++ {
			if rooms[r][c] == 0 {
				queue = append(queue, []int{r, c})
			}
		}
	}

	directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

	// Step 2: BFS
	for len(queue) > 0 {
		point := queue[0]
		queue = queue[1:]
		r, c := point[0], point[1]

		for _, dir := range directions {
			nr, nc := r+dir[0], c+dir[1]
			if nr < 0 || nc < 0 || nr >= rows || nc >= cols || rooms[nr][nc] != INF {
				continue
			}
			rooms[nr][nc] = rooms[r][c] + 1
			queue = append(queue, []int{nr, nc})
		}
	}
}
```

---

## 🧪 Sample Run

### Input:

```go
rooms := [][]int{
	{INF, -1, 0, INF},
	{INF, INF, INF, -1},
	{INF, -1, INF, -1},
	{0, -1, INF, INF},
}
wallsAndGates(rooms)
fmt.Println(rooms)
```

### Output:

```go
[
  [3, -1, 0, 1],
  [2,  2, 1, -1],
  [1, -1, 2, -1],
  [0, -1, 3, 4],
]
```

---

## 📌 Summary

| Concept | Description |
|--------|-------------|
| **Pattern** | Multi-source BFS |
| **When to use** | Multiple starting points spreading out |
| **Why BFS** | Because we need **minimum steps** (shortest distance) |
| **Trick** | Add all gates (0s) to the queue first |

---

Want me to generate a visual diagram for this? Or the DFS version (less efficient)?

## 📚 Summary Table

| Problem              | Grid Type       | Strategy      | Notes                                      |
|----------------------|------------------|---------------|--------------------------------------------|
| Flood Fill           | Color expansion | DFS/BFS       | Replace all connected cells                |
| Surrounded Regions   | Region capture  | DFS from edges| Mark safe regions, then flip the rest      |
| Walls and Gates      | Distance spread | Multi-source BFS | Fill shortest distances from gates      |

---
