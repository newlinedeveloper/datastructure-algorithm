Great! Let’s walk through the **CSES Problem 1667: Message Route**, which is a classic example of using **BFS in an unweighted, undirected graph** to find the **shortest path** from a source to a destination.

---

## 🧾 Problem Summary: Message Route (CSES 1667)

Given:
- `n` cities (nodes)
- `m` bidirectional roads (edges)
- You start in city `1` and want to reach city `n`.

Goal:
- Find the **shortest path** from city `1` to city `n`.
- If such a path exists, print its **length and the path itself**.
- Otherwise, print `IMPOSSIBLE`.

---

## 🔑 Key Concepts

- Since this is an **unweighted graph**, we use **BFS** for shortest path.
- We'll track the parent of each node to **reconstruct the path** after BFS.
- BFS guarantees the shortest path in an unweighted graph.

---

## 🧪 Example Input

```
5 5
1 2
1 3
2 4
3 5
4 5
```

### ✅ Output

```
3
1 2 4 5
```

---

## ✅ Go Code: BFS + Path Reconstruction

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

const MAXN = 100005

var (
	n, m    int
	graph   = make([][]int, MAXN)
	visited = make([]bool, MAXN)
	parent  = make([]int, MAXN)
)

func bfs(start int) {
	queue := []int{start}
	visited[start] = true
	parent[start] = -1

	for len(queue) > 0 {
		curr := queue[0]
		queue = queue[1:]

		for _, neighbor := range graph[curr] {
			if !visited[neighbor] {
				visited[neighbor] = true
				parent[neighbor] = curr
				queue = append(queue, neighbor)
			}
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

	bfs(1)

	if !visited[n] {
		fmt.Println("IMPOSSIBLE")
		return
	}

	// Reconstruct path
	path := []int{}
	for v := n; v != -1; v = parent[v] {
		path = append(path, v)
	}

	// Reverse path
	for i, j := 0, len(path)-1; i < j; i, j = i+1, j-1 {
		path[i], path[j] = path[j], path[i]
	}

	fmt.Println(len(path))
	for _, node := range path {
		fmt.Print(node, " ")
	}
	fmt.Println()
}
```

---

## 📊 Time and Space Complexity

- **Time Complexity**: `O(n + m)` – traverses each node and edge once.
- **Space Complexity**: `O(n + m)` – for graph, visited array, and parent tracking.

---

## 🔍 Why BFS?

- BFS finds the **shortest path** in terms of number of edges in an unweighted graph.
- DFS won't guarantee this, especially in cyclic graphs.

---

## 🧠 Takeaways

- For **unweighted graphs**, **always use BFS** for shortest path.
- Reconstruct the path using a **parent array**.
- Check if the destination was visited to determine reachability.

---

Want a diagram/visual walkthrough or extension to **finding all shortest paths**?
