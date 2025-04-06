Perfect! Let’s break down the **“Counting Rooms” (CSES 1192)** problem using **Graph Theory (DFS/Flood Fill)**.

---

## 🏢 Problem Summary – Counting Rooms

You are given a **2D grid** of size `n × m`, where each cell is either:
- `'.'` – empty space (can walk through)
- `'#'` – wall (cannot walk through)

Your task:  
👉 **Count the number of separate rooms** (connected components of `'.'`s).

---

## 🧠 Key Idea – Flood Fill (DFS on a Grid)

This is a classic **flood fill** / **DFS on a grid** problem.

We treat each `'.'` as a **node**, and adjacent `'.'` cells (up, down, left, right) as **edges**.

For every unvisited `'.'`, we start DFS and mark the entire connected region — and count it as **one room**.

---

## ✅ Step-by-Step Algorithm

1. Read the grid.
2. Create a `visited` matrix to keep track of cells.
3. Loop through each cell in the grid.
4. If it's `'.'` and not visited, start a DFS and **increment the room count**.
5. In DFS, recursively visit all 4 directions for `'.'` cells.

---

## 🧪 Example

**Input:**
```
n = 5, m = 8
########
#..#...#
####.#.#
#..#...#
########
```

**Output:**
```
3
```

(There are 3 distinct rooms)

---

## 🧾 Go Code – DFS Implementation

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

var (
	n, m   int
	grid   [][]rune
	visited [][]bool
	dx     = []int{-1, 1, 0, 0}
	dy     = []int{0, 0, -1, 1}
)

func dfs(x, y int) {
	visited[x][y] = true
	for i := 0; i < 4; i++ {
		nx, ny := x + dx[i], y + dy[i]
		if nx >= 0 && nx < n && ny >= 0 && ny < m {
			if !visited[nx][ny] && grid[nx][ny] == '.' {
				dfs(nx, ny)
			}
		}
	}
}

func main() {
	fmt.Scan(&n, &m)
	scanner := bufio.NewScanner(os.Stdin)
	grid = make([][]rune, n)
	visited = make([][]bool, n)

	scanner.Split(bufio.ScanLines)
	scanner.Buffer(make([]byte, 0, 4096), 1e6)

	for i := 0; i < n; i++ {
		scanner.Scan()
		line := scanner.Text()
		grid[i] = []rune(line)
		visited[i] = make([]bool, m)
	}

	roomCount := 0

	for i := 0; i < n; i++ {
		for j := 0; j < m; j++ {
			if grid[i][j] == '.' && !visited[i][j] {
				dfs(i, j)
				roomCount++
			}
		}
	}

	fmt.Println(roomCount)
}
```

---

## 🧠 Time & Space Complexity

- **Time Complexity:** O(n × m)  
  Each cell is visited once.

- **Space Complexity:** O(n × m)  
  For the `visited` matrix and recursion stack (in worst case).

---

## 🔄 Can we use BFS?

Yes! Replace DFS with a queue-based BFS — it works the same for flood fill.

---
