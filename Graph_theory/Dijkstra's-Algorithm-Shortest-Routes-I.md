Sure! Let’s dive into **Dijkstra’s Algorithm** using **CSES Problem Set: Shortest Routes I** (Problem ID: 1671). We’ll cover:

---

### 📌 **Problem Statement (CSES 1671 - Shortest Routes I)**

You are given a directed weighted graph with `n` nodes and `m` edges. Find the **shortest distance** from node `1` to all other nodes.

---

### ✅ **Constraints**
- 1 ≤ n ≤ 1e5
- 1 ≤ m ≤ 2e5
- Edge weights: 1 ≤ weight ≤ 1e9

This screams: **Dijkstra's Algorithm with a Min-Heap (Priority Queue)**.

---

### 📚 **Dijkstra's Algorithm Overview**

- Finds the shortest path from a source node to all other nodes in a **weighted graph with non-negative weights**.
- Efficient with a min-heap for selecting the current shortest node.

### 🧠 Dijkstra Core Idea:

1. Initialize all distances to ∞ except for the source node (`dist[1] = 0`).
2. Use a priority queue (min-heap) to explore the node with the current smallest distance.
3. For every neighboring node, check if the distance can be improved.
4. If yes, update and push to the heap.

---

### 🧪 **Test Input Example**
```
Input:
5 6
1 2 3
1 3 2
3 2 1
2 4 5
3 4 4
4 5 1

Output:
0 3 2 6 7
```

Explanation:
- Node 1 is the source.
- Shortest path to:
  - 2 is via 3: 1→3→2 = 2+1 = 3
  - 3 is direct: 2
  - 4 is via 3→2→4 = 2+1+5 = 8 → but better via 3→4 = 2+4 = 6
  - 5 is 1→3→4→5 = 2+4+1 = 7

---

### ✅ **Golang Code for Dijkstra (CSES 1671)**

```go
package main

import (
	"bufio"
	"container/heap"
	"fmt"
	"os"
)

type Edge struct {
	to, weight int
}

type Item struct {
	node, dist int
}

type PriorityQueue []Item

func (pq PriorityQueue) Len() int            { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool  { return pq[i].dist < pq[j].dist }
func (pq PriorityQueue) Swap(i, j int)       { pq[i], pq[j] = pq[j], pq[i] }
func (pq *PriorityQueue) Push(x interface{}) { *pq = append(*pq, x.(Item)) }
func (pq *PriorityQueue) Pop() interface{} {
	old := *pq
	n := len(old)
	item := old[n-1]
	*pq = old[:n-1]
	return item
}

func main() {
	reader := bufio.NewReader(os.Stdin)
	var n, m int
	fmt.Fscan(reader, &n, &m)

	graph := make([][]Edge, n+1)
	for i := 0; i < m; i++ {
		var a, b, c int
		fmt.Fscan(reader, &a, &b, &c)
		graph[a] = append(graph[a], Edge{to: b, weight: c})
	}

	const INF = 1 << 60
	dist := make([]int, n+1)
	for i := range dist {
		dist[i] = INF
	}
	dist[1] = 0

	pq := &PriorityQueue{}
	heap.Init(pq)
	heap.Push(pq, Item{node: 1, dist: 0})

	for pq.Len() > 0 {
		current := heap.Pop(pq).(Item)
		if current.dist > dist[current.node] {
			continue
		}
		for _, e := range graph[current.node] {
			if dist[e.to] > current.dist+e.weight {
				dist[e.to] = current.dist + e.weight
				heap.Push(pq, Item{node: e.to, dist: dist[e.to]})
			}
		}
	}

	writer := bufio.NewWriter(os.Stdout)
	defer writer.Flush()
	for i := 1; i <= n; i++ {
		fmt.Fprintf(writer, "%d ", dist[i])
	}
}
```

---

### ⏱️ **Time Complexity**

- `O((n + m) log n)` where:
  - `n` = number of nodes
  - `m` = number of edges
- Efficient for graphs with up to 1e5 nodes and 2e5 edges.

---

### ✅ Summary

| Component       | Explanation                                  |
|----------------|----------------------------------------------|
| Algorithm       | Dijkstra's Algorithm with Min-Heap           |
| Graph Type      | Directed weighted graph (positive weights)   |
| Data Structure  | Priority queue using Go's `container/heap`   |
| Time Complexity | O((n + m) log n)                             |
| Problem Link    | [CSES 1671 - Shortest Routes I](https://cses.fi/problemset/task/1671) |

---
