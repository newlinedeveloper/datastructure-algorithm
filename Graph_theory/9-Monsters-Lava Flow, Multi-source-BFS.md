The **"Monsters"** problem from [CSES (Problem 1194)](https://cses.fi/problemset/task/1194) involves **multi-source BFS** and is a variation of **grid pathfinding** where you avoid danger zones spreading over time — in this case, **monsters**.

---

## 🧟 Problem Summary

You’re given a grid with:

- `A` → Starting point of the **player**
- `M` → Starting points of **monsters**
- `#` → Walls (can’t be passed)
- `.` → Free cells

Monsters spread **simultaneously** in all directions one step per time unit. The player must find a way to escape the grid by **reaching any boundary cell** before a monster gets there.

---

### ✅ Your Task:

- Determine **if it's possible** for the player to escape.
- If so, output the path and its length.

---

## 🧠 Key Concepts

### 🔁 Multi-Source BFS:
Start a BFS from **all monster cells at once**, calculating the **earliest time each cell will be occupied by a monster**.

Then do **another BFS for the player**, ensuring the player never steps into a cell **at or after** the monster gets there.

---

## ✅ BFS Strategy

1. **Run BFS from all `M` positions** to fill a `monsterTime[][]` grid.
2. **Run BFS from `A`** to find the escape path:
   - Avoid `#`
   - Only move if `time[player] < time[monster]` at that cell.

---

## 🔧 Go Implementation

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

type point struct{ x, y int }

var (
	n, m int
	grid [][]byte

	dirs     = []point{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}
	dirChar  = []byte{'U', 'D', 'L', 'R'}
	monTime  [][]int
	playerPar [][]point
	visited  [][]bool
)

func inBounds(x, y int) bool {
	return x >= 0 && x < n && y >= 0 && y < m
}

func bfsMonsters(monsters []point) {
	queue := monsters
	for len(queue) > 0 {
		curr := queue[0]
		queue = queue[1:]
		for d := 0; d < 4; d++ {
			nx, ny := curr.x+dirs[d].x, curr.y+dirs[d].y
			if inBounds(nx, ny) && grid[nx][ny] != '#' && monTime[nx][ny] > monTime[curr.x][curr.y]+1 {
				monTime[nx][ny] = monTime[curr.x][curr.y] + 1
				queue = append(queue, point{nx, ny})
			}
		}
	}
}

func bfsPlayer(start point) (bool, point) {
	queue := []point{start}
	visited[start.x][start.y] = true
	time := make([][]int, n)
	for i := range time {
		time[i] = make([]int, m)
	}

	for len(queue) > 0 {
		curr := queue[0]
		queue = queue[1:]

		// If on boundary → escape
		if curr.x == 0 || curr.x == n-1 || curr.y == 0 || curr.y == m-1 {
			return true, curr
		}

		for d := 0; d < 4; d++ {
			nx, ny := curr.x+dirs[d].x, curr.y+dirs[d].y
			if inBounds(nx, ny) && !visited[nx][ny] && grid[nx][ny] != '#' && time[curr.x][curr.y]+1 < monTime[nx][ny] {
				visited[nx][ny] = true
				playerPar[nx][ny] = curr
				time[nx][ny] = time[curr.x][curr.y] + 1
				queue = append(queue, point{nx, ny})
			}
		}
	}
	return false, point{-1, -1}
}

func reconstructPath(end, start point) string {
	path := []byte{}
	curr := end
	for curr != start {
		prev := playerPar[curr.x][curr.y]
		for d := 0; d < 4; d++ {
			if prev.x+dirs[d].x == curr.x && prev.y+dirs[d].y == curr.y {
				path = append(path, dirChar[d])
				break
			}
		}
		curr = prev
	}
	// Reverse
	for i, j := 0, len(path)-1; i < j; i, j = i+1, j-1 {
		path[i], path[j] = path[j], path[i]
	}
	return string(path)
}

func main() {
	fmt.Scan(&n, &m)
	grid = make([][]byte, n)
	monTime = make([][]int, n)
	playerPar = make([][]point, n)
	visited = make([][]bool, n)

	var start point
	var monsters []point

	scanner := bufio.NewScanner(os.Stdin)
	for i := 0; i < n; i++ {
		grid[i] = make([]byte, m)
		monTime[i] = make([]int, m)
		playerPar[i] = make([]point, m)
		visited[i] = make([]bool, m)
		scanner.Scan()
		line := scanner.Bytes()
		for j := 0; j < m; j++ {
			grid[i][j] = line[j]
			if line[j] == 'M' {
				monsters = append(monsters, point{i, j})
				monTime[i][j] = 0
			} else {
				monTime[i][j] = 1 << 30
			}
			if line[j] == 'A' {
				start = point{i, j}
			}
		}
	}

	bfsMonsters(monsters)
	found, end := bfsPlayer(start)

	if found {
		fmt.Println("YES")
		path := reconstructPath(end, start)
		fmt.Println(len(path))
		fmt.Println(path)
	} else {
		fmt.Println("NO")
	}
}
```

---

## ✅ Sample Input

```
5 8
########
#M..A..#
#.##.#.#
#..M...#
########
```

### Output
```
YES
5
LURRD
```

---

## 📈 Time Complexity

- Monster BFS: O(n × m)
- Player BFS: O(n × m)
- Total: **O(n × m)**

---

## 🎯 Takeaways

- When **multiple sources** exist (e.g. monsters), use **multi-source BFS** to simulate their spread.
- Use BFS for shortest path in unweighted graphs (like grids).
- Keep track of **parent nodes** for path reconstruction.

Let me know if you’d like the same for similar problems like `Labyrinth`, `Counting Rooms`, or `Building Roads`.
