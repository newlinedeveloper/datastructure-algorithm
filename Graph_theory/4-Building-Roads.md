Great! Let's dive into **Graph 02: Building Roads**, which is a classic graph theory problem involving **Minimum Spanning Trees (MST)** using **DFS/BFS** or **Union-Find**.

---

## 🏗️ Problem: Building Roads (CSES Problem Set)

You are given `n` cities and a list of `m` existing roads (bidirectional edges). Your task is to determine:
1. The **minimum number of new roads** needed to connect all cities (i.e., make the graph connected).
2. The **actual roads** you should build (i.e., which pairs of cities to connect).

---

### 💡 Key Concepts:
- **Each connected component** represents a group of cities that are already connected.
- You need to **connect all disconnected components** using new roads.
- If there are `k` components, you need at least `k-1` roads to connect them.

---

## ✅ Strategy:
1. Use **DFS/BFS** to find all **connected components**.
2. Pick **one representative city from each component**.
3. Connect these representatives in a chain to ensure full connectivity.
4. Output the count and the roads you add.

---

## ✍️ Example:

**Input:**
```
6 3
1 2
2 3
4 5
```

**Explanation:**
- Component 1: 1–2–3
- Component 2: 4–5
- Component 3: 6

**Output:**
```
2
3 4
5 6
```

We added roads from city 3 to 4 and 5 to 6 to connect everything.

---

## 🧾 Go Code:

```go
package main

import (
	"fmt"
)

var (
	n, m int
	graph [][]int
	visited []bool
)

func dfs(node int) {
	visited[node] = true
	for _, neighbor := range graph[node] {
		if !visited[neighbor] {
			dfs(neighbor)
		}
	}
}

func main() {
	fmt.Scan(&n, &m)
	graph = make([][]int, n+1)
	visited = make([]bool, n+1)

	for i := 0; i < m; i++ {
		var a, b int
		fmt.Scan(&a, &b)
		graph[a] = append(graph[a], b)
		graph[b] = append(graph[b], a)
	}

	var representatives []int

	for i := 1; i <= n; i++ {
		if !visited[i] {
			representatives = append(representatives, i)
			dfs(i)
		}
	}

	fmt.Println(len(representatives) - 1)
	for i := 1; i < len(representatives); i++ {
		fmt.Println(representatives[i-1], representatives[i])
	}
}
```

---

## 🧠 Time & Space Complexity:

- **Time:** O(n + m) — Standard DFS traversal
- **Space:** O(n + m) — Adjacency list + visited array

---

## 🔄 Alternate Approaches:
- You could also solve this using **Union-Find (Disjoint Set Union)** to find connected components.
- But DFS is easier and intuitive here since edge weights are not relevant.

---

Would you like the **Union-Find version** too, or should we visualize the components and connections step by step?
