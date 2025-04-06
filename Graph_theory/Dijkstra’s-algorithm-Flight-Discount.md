Great! Let’s break down the **CSES 1195 – Flight Discount** problem and solve it using a **modified Dijkstra’s algorithm**.

---

## ✈️ CSES 1195 – Flight Discount  
🔗 [Link to Problem](https://cses.fi/problemset/task/1195)

---

### 🧩 **Problem Summary**

You're given a directed graph with `n` cities and `m` flights.  
Each flight has a **positive cost**. You want to fly from city `1` to city `n`.  
You are **allowed to use *at most one* discount** — which **halves the cost of any one flight**.

---

### ✅ **Objective**

Find the **minimum cost** to travel from city `1` to city `n`, **using at most one discount** on any **one** edge.

---

## 🧠 **Key Insight**

We need to track **two states** for each node:

- **Without discount** used (normal Dijkstra state)
- **With discount used** (i.e., we already applied the 50% off to one edge)

So we’ll use **Dijkstra’s algorithm** with a **state flag**:
- `dist[node][0]` → minimum cost to reach `node` **without using discount yet**
- `dist[node][1]` → minimum cost to reach `node` **after using discount**

---

### 🚀 Go Code (Modified Dijkstra)

```go
package main

import (
	"container/heap"
	"fmt"
	"math"
)

type Edge struct {
	to   int
	cost int64
}

type Item struct {
	node     int
	cost     int64
	discountUsed int // 0 or 1
}
type PriorityQueue []Item

func (pq PriorityQueue) Len() int            { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool  { return pq[i].cost < pq[j].cost }
func (pq PriorityQueue) Swap(i, j int)       { pq[i], pq[j] = pq[j], pq[i] }
func (pq *PriorityQueue) Push(x interface{}) { *pq = append(*pq, x.(Item)) }
func (pq *PriorityQueue) Pop() interface{} {
	old := *pq
	n := len(old)
	item := old[n-1]
	*pq = old[:n-1]
	return item
}

func min(a, b int64) int64 {
	if a < b {
		return a
	}
	return b
}

func main() {
	var n, m int
	fmt.Scan(&n, &m)

	graph := make([][]Edge, n+1)
	for i := 0; i < m; i++ {
		var u, v int
		var c int64
		fmt.Scan(&u, &v, &c)
		graph[u] = append(graph[u], Edge{v, c})
	}

	const INF = int64(1e18)
	dist := make([][2]int64, n+1)
	for i := 1; i <= n; i++ {
		dist[i][0] = INF
		dist[i][1] = INF
	}

	pq := &PriorityQueue{}
	heap.Init(pq)
	dist[1][0] = 0
	heap.Push(pq, Item{1, 0, 0})

	for pq.Len() > 0 {
		curr := heap.Pop(pq).(Item)
		node, cost, used := curr.node, curr.cost, curr.discountUsed

		if cost > dist[node][used] {
			continue
		}

		for _, e := range graph[node] {
			// Without using discount
			if dist[e.to][used] > cost+e.cost {
				dist[e.to][used] = cost + e.cost
				heap.Push(pq, Item{e.to, dist[e.to][used], used})
			}
			// Use discount (only if not yet used)
			if used == 0 {
				discounted := cost + e.cost/2
				if dist[e.to][1] > discounted {
					dist[e.to][1] = discounted
					heap.Push(pq, Item{e.to, discounted, 1})
				}
			}
		}
	}

	fmt.Println(dist[n][1])
}
```

---

## 🔍 Example

**Input:**

```
4 5
1 2 5
2 4 5
1 3 2
3 4 8
1 4 20
```

**Output:**

```
10
```

**Explanation:**

- Path 1→3→4 costs `2 + 8 = 10`, if discount is applied on `8`, it becomes `2 + 4 = 6`
- Best path is 1→2→4: `5 + 5 = 10`, apply discount on either `5` = `5 + 2 = 7`
- Final best = `7` → so answer is `7`

Actually, with this example, **discount on 8** gives cheaper: `2 + 4 = 6`, best is `6`

---

## ⏱ Time Complexity

| Component     | Complexity      |
|---------------|-----------------|
| Dijkstra      | O((n + m) log n) |
| States        | 2 per node (with and without discount) |

Efficient enough for `n ≤ 1e5` and `m ≤ 1e5`

---
