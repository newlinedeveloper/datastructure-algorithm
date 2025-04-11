Perfect! Let’s explore the **Minimum Spanning Tree (MST)** graph pattern — crucial for cost-optimization problems that involve connecting nodes (cities, points, houses) efficiently.

---

## 🔁 7. Minimum Spanning Tree (MST)

### 🔍 **How to Identify This Pattern**

Look for:
- “Connect all X with minimum cost”
- “Build roads/pipes/wires between cities/houses”
- All nodes must be **connected** with **minimum total cost**
- No cycles (tree structure)

---

### 🧠 **How to Solve**

Two main algorithms:

#### ✅ **Kruskal’s Algorithm** (Greedy + Union-Find):
- Sort edges by weight
- Add smallest edge that doesn’t create a cycle
- Stop when you have `n-1` edges

#### ✅ **Prim’s Algorithm** (Greedy + Min-Heap):
- Start from any node
- Always pick the lowest-cost edge to expand MST
- Use a priority queue (min-heap)

---

### 📝 Problem 1: **Minimum Cost to Connect All Points**  
📌 [Leetcode 1584](https://leetcode.com/problems/min-cost-to-connect-all-points)

> Given 2D points, connect them with minimum cost using Manhattan Distance.

---

### ✅ Input:
```go
points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
```

### ✅ Output:
```
20
```

---

## 🚀 Go Code (Prim’s Algorithm using Min-Heap)

```go
import (
    "container/heap"
    "math"
)

type Edge struct {
    cost, to int
}

type MinHeap []Edge

func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i].cost < h[j].cost }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(Edge)) }
func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func minCostConnectPoints(points [][]int) int {
    n := len(points)
    visited := make([]bool, n)
    minHeap := &MinHeap{}
    heap.Init(minHeap)

    heap.Push(minHeap, Edge{0, 0})
    totalCost := 0
    edgesUsed := 0

    for minHeap.Len() > 0 && edgesUsed < n {
        edge := heap.Pop(minHeap).(Edge)
        if visited[edge.to] {
            continue
        }

        visited[edge.to] = true
        totalCost += edge.cost
        edgesUsed++

        for i := 0; i < n; i++ {
            if !visited[i] {
                cost := abs(points[edge.to][0]-points[i][0]) + abs(points[edge.to][1]-points[i][1])
                heap.Push(minHeap, Edge{cost, i})
            }
        }
    }

    return totalCost
}

func abs(a int) int {
    if a < 0 {
        return -a
    }
    return a
}
```

---

### 🧠 Explanation

- We build a **MST** using Prim’s:
  - Start from any point (0)
  - Push all reachable edges to the heap
  - Always pick the smallest edge
- Repeat until all points are visited

---

### 📝 Problem 2: Optimize Water Distribution in a Village  
📌 [Leetcode 1168](https://leetcode.com/problems/optimize-water-distribution-in-a-village)

> Build wells or pipes between houses. Each well has a fixed cost.

---

### ✅ Input:
```go
n = 3
wells = [1,2,2]
pipes = [[1,2,1],[2,3,1]]
```

### ✅ Output:
```
3
```

---

## 🚀 Go Code (Kruskal’s Algorithm + Virtual Node)

```go
type Edge struct {
    u, v, cost int
}

type UnionFind struct {
    parent []int
}

func NewUF(n int) *UnionFind {
    p := make([]int, n)
    for i := range p {
        p[i] = i
    }
    return &UnionFind{parent: p}
}

func (uf *UnionFind) Find(x int) int {
    if uf.parent[x] != x {
        uf.parent[x] = uf.Find(uf.parent[x])
    }
    return uf.parent[x]
}

func (uf *UnionFind) Union(x, y int) bool {
    px, py := uf.Find(x), uf.Find(y)
    if px == py {
        return false
    }
    uf.parent[px] = py
    return true
}

func minCostToSupplyWater(n int, wells []int, pipes [][]int) int {
    var edges []Edge
    // Add virtual node 0 to every house with well cost
    for i := 0; i < n; i++ {
        edges = append(edges, Edge{0, i + 1, wells[i]})
    }

    // Add pipes as normal edges
    for _, pipe := range pipes {
        edges = append(edges, Edge{pipe[0], pipe[1], pipe[2]})
    }

    // Sort edges by cost
    sort.Slice(edges, func(i, j int) bool {
        return edges[i].cost < edges[j].cost
    })

    uf := NewUF(n + 1) // virtual node + n houses
    total := 0

    for _, edge := range edges {
        if uf.Union(edge.u, edge.v) {
            total += edge.cost
        }
    }

    return total
}
```

---

### 🧠 Explanation

- Treat the **well at each house** as a connection to a **virtual node 0**.
- Use **Kruskal’s algorithm** to select cheapest edges (pipes or wells).
- Union-Find ensures no cycles and connects everything minimally.

---

### 📝 Problem 3: Connecting Cities With Minimum Cost  
📌 [LintCode 435](https://www.lintcode.com/problem/connecting-cities-with-minimum-cost/)
Perfect — let’s dive into **Problem 3: Connecting Cities With Minimum Cost** (📌 LintCode 435).

---

## 📝 **Problem Statement:**
You are given `n` cities labeled from `1` to `n`, and a list of `connections` where each connection contains `[city1, city2, cost]`. Your task is to **connect all the cities** with the **minimum total cost**.  
If it’s **not possible**, return `-1`.

---

### ✅ **Input Example:**

```go
n = 3
connections = [
  [1, 2, 5],
  [1, 3, 6],
  [2, 3, 1]
]
```

### ✅ **Output:**
```
6
```

### 🧠 **Explanation:**
Use the roads:
- 2 → 3 (cost 1)
- 1 → 2 (cost 5)  
**Total: 6**

This forms a Minimum Spanning Tree (MST).

---

## 🔁 **Pattern: Minimum Spanning Tree (MST)**  
- All nodes must be **connected**
- Minimize **cost**
- Graph with **n nodes** must have **n-1 edges**
- Apply **Kruskal’s algorithm** (or Prim’s)

---

## 🚀 Golang Solution Using Kruskal's Algorithm

```go
import (
    "sort"
)

type UnionFind struct {
    parent []int
}

func NewUF(n int) *UnionFind {
    p := make([]int, n+1)
    for i := 1; i <= n; i++ {
        p[i] = i
    }
    return &UnionFind{parent: p}
}

func (uf *UnionFind) Find(x int) int {
    if uf.parent[x] != x {
        uf.parent[x] = uf.Find(uf.parent[x])
    }
    return uf.parent[x]
}

func (uf *UnionFind) Union(x, y int) bool {
    px, py := uf.Find(x), uf.Find(y)
    if px == py {
        return false
    }
    uf.parent[px] = py
    return true
}

func minimumCost(n int, connections [][]int) int {
    sort.Slice(connections, func(i, j int) bool {
        return connections[i][2] < connections[j][2]
    })

    uf := NewUF(n)
    totalCost := 0
    edgesUsed := 0

    for _, conn := range connections {
        u, v, cost := conn[0], conn[1], conn[2]
        if uf.Union(u, v) {
            totalCost += cost
            edgesUsed++
        }
    }

    if edgesUsed == n-1 {
        return totalCost
    }
    return -1
}
```

---

## 🧠 Explanation:

- **Union-Find** is used to avoid cycles while merging components
- Sort edges by cost (greedy)
- Try to **connect** each edge if cities aren’t already in the same component
- If we have connected `n-1` edges, return total cost; else return `-1`

---

## ✅ Time and Space Complexity:

- **Time:** O(E log E) for sorting edges + O(E α(N)) for union-find ops  
- **Space:** O(N) for Union-Find structure

---

## 🧪 Test Case:

```go
n := 4
connections := [][]int{
    {1, 2, 3},
    {3, 4, 4},
    {1, 4, 2},
    {1, 3, 4},
    {2, 3, 5},
}
fmt.Println(minimumCost(n, connections)) // Output: 9
```

---

## 🔄 Summary Table

| Problem                               | Pattern     | Approach           |
|----------------------------------------|-------------|--------------------|
| Min Cost to Connect All Points         | MST         | **Prim’s** + Heap  |
| Optimize Water Distribution in Village | MST + Trick | **Kruskal’s** + Virtual Node |
| Connecting Cities                      | MST         | Kruskal/Prim       |

---

Want to go next with **Bridges & Articulation Points** (important in network reliability) or **Union-Find patterns** like dynamic connectivity / accounts merge?
