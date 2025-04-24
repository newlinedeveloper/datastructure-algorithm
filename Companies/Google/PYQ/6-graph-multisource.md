This is a classic graph problem with a twist. You're given:

- An **undirected, unweighted graph**.
- Alice starts at node `A`, Bob at node `B`.
- Both need to reach a destination node `D`.
- Alice and Bob can **share paths**, and we need to **minimize the number of distinct edges** used **collectively** by both paths.

---

### 🔍 Key Idea

Instead of just computing the paths separately for Alice and Bob, the goal is to **share as much of the path as possible**, especially near the destination, so that the **total distinct edges used is minimized**.

---

### ✅ Solution Strategy

1. **Run BFS from each of A, B, and D** to compute shortest distances from them to all nodes.
2. For every node `X`, calculate:
   - Distance from `A` to `X` → `da`
   - Distance from `B` to `X` → `db`
   - Distance from `X` to `D` → `dx`
   - Total edges used = `da + db + dx`
3. The idea is that if both Alice and Bob meet at node `X`, and go together to `D`, we **share** the path from `X` to `D` instead of duplicating it.
4. Pick the `X` that gives minimum `da + db + dx`.

---

### ✅ Golang Code

```go
package main

import (
	"fmt"
)

func bfs(graph map[string][]string, start string) map[string]int {
	dist := make(map[string]int)
	visited := make(map[string]bool)
	queue := []string{start}
	dist[start] = 0
	visited[start] = true

	for len(queue) > 0 {
		curr := queue[0]
		queue = queue[1:]
		for _, neighbor := range graph[curr] {
			if !visited[neighbor] {
				dist[neighbor] = dist[curr] + 1
				queue = append(queue, neighbor)
				visited[neighbor] = true
			}
		}
	}
	return dist
}

func minDistinctEdges(graph map[string][]string, alice, bob, dest string) int {
	da := bfs(graph, alice)
	db := bfs(graph, bob)
	dd := bfs(graph, dest)

	minTotal := 1 << 30 // Infinity
	for node := range graph {
		// Check if all distances are defined (i.e. reachable)
		if d1, ok1 := da[node]; ok1 {
			if d2, ok2 := db[node]; ok2 {
				if d3, ok3 := dd[node]; ok3 {
					total := d1 + d2 + d3
					if total < minTotal {
						minTotal = total
					}
				}
			}
		}
	}
	return minTotal
}

func main() {
	graph := map[string][]string{
		"A": {"F", "C"},
		"B": {"C", "G"},
		"C": {"A", "B", "E"},
		"E": {"C", "D"},
		"F": {"A", "D"},
		"D": {"F", "E"},
		"G": {"B", "H"},
		"H": {"G"},
	}

	alice := "A"
	bob := "B"
	dest := "D"

	fmt.Println("Minimum distinct edges used:", minDistinctEdges(graph, alice, bob, dest))
}
```

---

### 🧠 Explanation of Output

- Alice: `A → C → E → D` (3 edges)
- Bob:   `B → C → E → D` (3 edges)
- **Common subpath**: `C → E → D` (2 edges)

So:
- Total distinct edges = `A → C` + `B → C` + `C → E` + `E → D` = **4**

And that’s the minimum!

---

### ⏱️ Time Complexity

- BFS: O(N + E) per traversal
- 3 BFS runs = O(3 * (N + E)) = O(N + E)
- Final loop over nodes: O(N)

So total: **O(N + E)**

---

Let me know if you want to visualize the paths or test with a different graph!
