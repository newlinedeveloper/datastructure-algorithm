Let's break down the **CSES Problem 1193: Labyrinth**, which is another **BFS on a 2D grid** problem—very similar in idea to pathfinding in mazes.

---

## 🔍 Problem Statement: Labyrinth (CSES 1193)

Given a 2D grid (`n x m`) consisting of:

- `#` → Wall
- `.` → Open space
- `A` → Starting position
- `B` → Destination

Your task:
- Determine if it's possible to reach `B` from `A`.
- If yes, print `YES`, followed by the **length of the shortest path**, and the actual path using directions:
  - `U`, `D`, `L`, `R`

---

## 📥 Sample Input

```
5 8
########
#A...#B#
#.##.#.#
#......#
########
```

### ✅ Output
```
YES
9
RRRRDDDLL
```

---

## 💡 Approach

- **BFS** is ideal for shortest path in **unweighted graphs** — here, each grid cell is a node.
- Use a queue to perform BFS from `A`.
- Track:
  - `visited` status
  - `from` direction (to reconstruct path)
  - Parent cells

---

## ✅ Go Implementation

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

type point struct {
	x, y int
}

var (
	n, m    int
	grid    [][]byte
	visited [][]bool
	parent  [][]point
	move    = []point{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}
	dir     = []byte{'U', 'D', 'L', 'R'}
)

func inBounds(x, y int) bool {
	return x >= 0 && x < n && y >= 0 && y < m
}

func bfs(start, end point) bool {
	queue := []point{start}
	visited[start.x][start.y] = true

	for len(queue) > 0 {
		curr := queue[0]
		queue = queue[1:]

		if curr == end {
			return true
		}

		for d := 0; d < 4; d++ {
			nx := curr.x + move[d].x
			ny := curr.y + move[d].y

			if inBounds(nx, ny) && !visited[nx][ny] && grid[nx][ny] != '#' {
				visited[nx][ny] = true
				parent[nx][ny] = curr
				queue = append(queue, point{nx, ny})
			}
		}
	}
	return false
}

func main() {
	fmt.Scan(&n, &m)
	grid = make([][]byte, n)
	visited = make([][]bool, n)
	parent = make([][]point, n)

	var start, end point

	scanner := bufio.NewScanner(os.Stdin)
	for i := 0; i < n; i++ {
		grid[i] = make([]byte, m)
		visited[i] = make([]bool, m)
		parent[i] = make([]point, m)
		scanner.Scan()
		row := scanner.Bytes()
		for j := 0; j < m; j++ {
			grid[i][j] = row[j]
			if grid[i][j] == 'A' {
				start = point{i, j}
			}
			if grid[i][j] == 'B' {
				end = point{i, j}
			}
		}
	}

	if bfs(start, end) {
		fmt.Println("YES")
		path := []byte{}
		curr := end
		for curr != start {
			prev := parent[curr.x][curr.y]
			for d := 0; d < 4; d++ {
				if prev.x+move[d].x == curr.x && prev.y+move[d].y == curr.y {
					path = append(path, dir[d])
					break
				}
			}
			curr = prev
		}
		// Reverse path
		for i, j := 0, len(path)-1; i < j; i, j = i+1, j-1 {
			path[i], path[j] = path[j], path[i]
		}
		fmt.Println(len(path))
		fmt.Println(string(path))
	} else {
		fmt.Println("NO")
	}
}
```

---

## 📊 Time and Space Complexity

- **Time**: `O(n * m)` — each cell is visited once.
- **Space**: `O(n * m)` — for visited, parent, and queue storage.

---

## ✅ Takeaways

- This is a classic **grid BFS** shortest path problem.
- To reconstruct the path, use a **parent array** and backtrack.
- Use direction arrays to simplify moving in 4 directions and storing steps.

---
