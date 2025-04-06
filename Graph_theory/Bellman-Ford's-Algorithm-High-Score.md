Let's break down **Bellman-Ford’s Algorithm** using the problem **CSES 1673 - High Score**.

---

## 🚩 Problem: [High Score (CSES 1673)](https://cses.fi/problemset/task/1673)

You're given a **directed graph** with `n` nodes and `m` edges. Some edge weights may be **negative**. You need to find the **maximum score** possible by going from node `1` to node `n`.

But there's a twist:  
> If the score can be made **arbitrarily large** (i.e., **positive cycle** is reachable), then print `-1`.

---

## 🔍 Key Observations

- You need **maximum score**, so you must **maximize path weights**.
- But there may be **positive cycles** (e.g., cycles with negative weight when inverted).
- So: Use **Bellman-Ford Algorithm**.
- Bellman-Ford can detect **negative weight cycles** (we’ll invert signs to detect positive cycles).

---

## 🧠 Approach

### ✅ Step 1: Invert edge weights

Since Bellman-Ford finds the **shortest path**, and we want to **maximize score**, we:
- Invert all edge weights: `w → -w`.

### ✅ Step 2: Run Bellman-Ford

- Initialize all distances to `INF`, `dist[1] = 0`.
- Do `n-1` relaxations:
  - For every edge `(u, v, w)`, update `dist[v] = min(dist[v], dist[u] + w)`.

### ✅ Step 3: Detect negative cycle

- Run 1 more iteration.
- If any distance improves, **there is a negative cycle** in the inverted graph → a **positive cycle** in original graph.

---

## ✨ Special Condition

Even if a cycle exists, we only care if it’s:
- **Reachable from node 1**, and
- Can reach **node n**.

So we use:
- DFS from `1` → mark reachable nodes.
- Reverse DFS from `n` → mark nodes that can reach `n`.

Only cycles that are **on a path from 1 to n** are considered.

---

## ✅ Go Code for Bellman-Ford (CSES 1673)

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

type Edge struct {
	from, to, weight int64
}

var (
	n, m    int64
	edges   []Edge
	graph   [][]int64
	revGraph [][]int64
	visitedFromStart []bool
	visitedFromEnd   []bool
)

func dfs(u int64, visited []bool, g [][]int64) {
	visited[u] = true
	for _, v := range g[u] {
		if !visited[v] {
			dfs(v, visited, g)
		}
	}
}

func main() {
	reader := bufio.NewReader(os.Stdin)
	fmt.Fscan(reader, &n, &m)

	edges = make([]Edge, m)
	graph = make([][]int64, n+1)
	revGraph = make([][]int64, n+1)

	for i := int64(0); i < m; i++ {
		var u, v, w int64
		fmt.Fscan(reader, &u, &v, &w)
		edges[i] = Edge{from: u, to: v, weight: -w} // invert weight for max path
		graph[u] = append(graph[u], v)
		revGraph[v] = append(revGraph[v], u) // for reverse DFS
	}

	dist := make([]int64, n+1)
	const INF int64 = 1e18
	for i := int64(1); i <= n; i++ {
		dist[i] = INF
	}
	dist[1] = 0

	// Bellman-Ford (n-1 iterations)
	for i := int64(0); i < n-1; i++ {
		for _, e := range edges {
			if dist[e.from] < INF && dist[e.from]+e.weight < dist[e.to] {
				dist[e.to] = dist[e.from] + e.weight
			}
		}
	}

	// Check for negative cycle (positive cycle in original)
	bad := make([]bool, n+1)
	for _, e := range edges {
		if dist[e.from] < INF && dist[e.from]+e.weight < dist[e.to] {
			bad[e.to] = true
		}
	}

	// Run DFS from node 1 and to node n (reverse)
	visitedFromStart = make([]bool, n+1)
	visitedFromEnd = make([]bool, n+1)
	dfs(1, visitedFromStart, graph)
	dfs(n, visitedFromEnd, revGraph)

	// If bad node is on path from 1 to n
	for i := int64(1); i <= n; i++ {
		if bad[i] && visitedFromStart[i] && visitedFromEnd[i] {
			fmt.Println(-1)
			return
		}
	}

	// Output max score (inverted sign)
	fmt.Println(-dist[n])
}
```

---

## 🧪 Sample Input

```
5 6
1 2 2
2 3 3
3 4 -5
4 2 1
3 5 2
1 5 100
```

### 🔢 Output
```
100
```

But if we changed the cycle to be positive in original graph, like:

```
3 4 -10
```

→ Output: `-1` (cycle used to increase score infinitely).

---

## ⏱ Time Complexity

| Step           | Complexity    |
|----------------|---------------|
| Bellman-Ford   | O(n * m)      |
| DFS/Rev-DFS    | O(n + m)      |
| Total          | O(n * m)      |

Works for `n ≤ 1e5`, `m ≤ 2e5`.

---

## ✅ Summary

| Concept            | Used                                  |
|--------------------|---------------------------------------|
| Algorithm          | Bellman-Ford                          |
| Detect Cycle       | Yes, using extra iteration            |
| Handle +ve Cycle   | By inverting weights                  |
| Cycle on path 1→n  | DFS from 1 and reverse DFS from n     |
| Output             | `-dist[n]` if valid, else `-1`        |

---
