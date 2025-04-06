Great choice! Let's walk through **CSES Problem 1668: Building Teams**, which is a **Graph Coloring / Bipartite Graph** problem.

---

## 🧠 Problem Summary: **Building Teams (CSES 1668)**

You are given an **undirected graph** representing `n` students and `m` friendships between them. Your task is to divide all students into **two teams** such that **no two friends are on the same team**.

If it’s not possible, print `"IMPOSSIBLE"`. Otherwise, print the team assignment (either `1` or `2` for each student).

---

## 💡 Key Concepts

- This is a **graph coloring problem** — you need to **2-color** the graph.
- The graph is **bipartite** iff it can be colored using two colors such that no two adjacent nodes have the same color.
- Use either **DFS** or **BFS** to color the graph.

---

## ✍️ Sample Input
```
4 4
1 2
1 3
2 4
3 4
```

### ➕ Explanation:
- This graph is a square (cycle of even length), so it's bipartite.
- Possible coloring:  
  - Team 1: 1, 4  
  - Team 2: 2, 3

---

## ✅ Strategy:
1. Represent the graph using an adjacency list.
2. Initialize a `color` array of size `n+1` with all `0` (uncolored).
3. For each unvisited node, use BFS/DFS to try assigning colors (1 or 2).
4. If a conflict is detected (same color for neighbors), output `"IMPOSSIBLE"`.

---

## ✅ Golang Code (DFS-based):

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

const MAXN = 100005

var (
	n, m   int
	graph  = make([][]int, MAXN)
	color  = make([]int, MAXN) // 0 = unvisited, 1 or 2 = team
	possible = true
)

func dfs(node, c int) {
	color[node] = c
	for _, neighbor := range graph[node] {
		if color[neighbor] == 0 {
			dfs(neighbor, 3-c)
		} else if color[neighbor] == color[node] {
			possible = false
			return
		}
	}
}

func main() {
	scanner := bufio.NewScanner(os.Stdin)
	scanner.Split(bufio.ScanWords)

	readInt := func() int {
		scanner.Scan()
		val := 0
		for _, b := range scanner.Bytes() {
			val = val*10 + int(b-'0')
		}
		return val
	}

	n = readInt()
	m = readInt()

	for i := 0; i < m; i++ {
		u := readInt()
		v := readInt()
		graph[u] = append(graph[u], v)
		graph[v] = append(graph[v], u)
	}

	for i := 1; i <= n; i++ {
		if color[i] == 0 {
			dfs(i, 1)
		}
	}

	if !possible {
		fmt.Println("IMPOSSIBLE")
		return
	}

	for i := 1; i <= n; i++ {
		fmt.Print(color[i], " ")
	}
}
```

---

## 🧠 Time Complexity:
- **O(n + m)** — Each node and edge is visited once.

## 💾 Space Complexity:
- **O(n + m)** — Graph adjacency list + color array.

---

## 🔄 Alternate BFS Approach:
- Use BFS to alternate colors level by level.
- Safer for **very deep trees** to avoid stack overflow.
