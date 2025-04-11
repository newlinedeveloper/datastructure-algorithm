Let’s dive into the **Shortest Path** pattern in Graphs — another **high-impact** area in interviews, especially for Google. It comes up in path planning, routing systems, networking, and more.

---

## 🔁 3. Shortest Path Pattern

### 🔍 How to Identify This Pattern

Look for keywords like:
- "Shortest path", "Minimum steps", "Least time/cost", "Fewest moves"
- The graph may be **unweighted**, **weighted (positive/negative)**, or may include **constraints (like K stops)**

### 🧠 Algorithm Choices

| Graph Type                            | Use            |
|--------------------------------------|----------------|
| Unweighted / Equal-cost Edges        | **BFS**        |
| Weighted + Non-negative Edges        | **Dijkstra’s** |
| Weighted + Can Include Negative Edges| **Bellman-Ford** |

---

## 🔹 Problem 1: **Network Delay Time**  
📌 [Leetcode](https://leetcode.com/problems/network-delay-time/)  
Given a list of travel times `times[i] = (u, v, w)`, return how long it takes for all nodes to receive the signal sent from node `k`.

### ✅ Sample Input:
```go
times = [[2,1,1],[2,3,1],[3,4,1]]
n = 4
k = 2
```
### ✅ Output:
```go
2
```

---

### 🚀 Go Code (Dijkstra’s Algorithm):

```go
type Pair struct {
    node, time int
}

func networkDelayTime(times [][]int, n int, k int) int {
    graph := make(map[int][][2]int)
    for _, t := range times {
        u, v, w := t[0], t[1], t[2]
        graph[u] = append(graph[u], [2]int{v, w})
    }

    dist := make([]int, n+1)
    for i := range dist {
        dist[i] = math.MaxInt32
    }
    dist[k] = 0

    pq := &PriorityQueue{}
    heap.Init(pq)
    heap.Push(pq, [2]int{k, 0}) // (node, time)

    for pq.Len() > 0 {
        top := heap.Pop(pq).([2]int)
        u, time := top[0], top[1]

        if time > dist[u] {
            continue
        }

        for _, next := range graph[u] {
            v, w := next[0], next[1]
            if dist[v] > time+w {
                dist[v] = time + w
                heap.Push(pq, [2]int{v, dist[v]})
            }
        }
    }

    maxTime := 0
    for i := 1; i <= n; i++ {
        if dist[i] == math.MaxInt32 {
            return -1
        }
        maxTime = max(maxTime, dist[i])
    }

    return maxTime
}

// PriorityQueue implements heap.Interface
type PriorityQueue [][2]int

func (pq PriorityQueue) Len() int            { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool  { return pq[i][1] < pq[j][1] }
func (pq PriorityQueue) Swap(i, j int)       { pq[i], pq[j] = pq[j], pq[i] }
func (pq *PriorityQueue) Push(x interface{}) { *pq = append(*pq, x.([2]int)) }
func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    x := old[n-1]
    *pq = old[:n-1]
    return x
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### 🧠 Explanation:
- Dijkstra's algorithm with min-heap (priority queue).
- Tracks the minimum time to reach each node.
- Returns the maximum of all shortest times.

---

## 🔹 Problem 2: **Cheapest Flights Within K Stops**  
📌 [Leetcode](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

### ✅ Input:
```go
n = 4, flights = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 1
```
### ✅ Output:
```go
200
```

---

### 🚀 Go Code (BFS with (node, cost, stops)):
```go
func findCheapestPrice(n int, flights [][]int, src int, dst int, k int) int {
    graph := make(map[int][][2]int)
    for _, f := range flights {
        graph[f[0]] = append(graph[f[0]], [2]int{f[1], f[2]})
    }

    type Node struct {
        city, cost, stops int
    }

    queue := []Node{{src, 0, 0}}
    minCost := make([]int, n)
    for i := range minCost {
        minCost[i] = math.MaxInt32
    }
    minCost[src] = 0

    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]

        if node.stops > k {
            continue
        }

        for _, neighbor := range graph[node.city] {
            nextCity, price := neighbor[0], neighbor[1]
            newCost := node.cost + price
            if newCost < minCost[nextCity] {
                minCost[nextCity] = newCost
                queue = append(queue, Node{nextCity, newCost, node.stops + 1})
            }
        }
    }

    if minCost[dst] == math.MaxInt32 {
        return -1
    }
    return minCost[dst]
}
```

### 🧠 Explanation:
- Use BFS with `stops` count to limit traversal to `K+1` layers.
- Only update cost if it's better than current min.

---

## 🔹 Problem 3: **Shortest Path in Binary Matrix**  
📌 [Leetcode](https://leetcode.com/problems/shortest-path-in-binary-matrix/)  
Find shortest clear path from top-left to bottom-right in a binary matrix.

### ✅ Sample Input:
```go
grid = [
  [0,1],
  [1,0]
]
```
### ✅ Output:
```go
2
```

---

### 🚀 Go Code (BFS in 8 directions):
```go
func shortestPathBinaryMatrix(grid [][]int) int {
    n := len(grid)
    if grid[0][0] != 0 || grid[n-1][n-1] != 0 {
        return -1
    }

    dirs := [8][2]int{
        {1, 0}, {-1, 0}, {0, 1}, {0, -1},
        {1, 1}, {-1, -1}, {1, -1}, {-1, 1},
    }

    queue := [][3]int{{0, 0, 1}} // x, y, distance
    visited := make([][]bool, n)
    for i := range visited {
        visited[i] = make([]bool, n)
    }
    visited[0][0] = true

    for len(queue) > 0 {
        x, y, dist := queue[0][0], queue[0][1], queue[0][2]
        queue = queue[1:]

        if x == n-1 && y == n-1 {
            return dist
        }

        for _, d := range dirs {
            nx, ny := x+d[0], y+d[1]
            if nx >= 0 && ny >= 0 && nx < n && ny < n &&
                !visited[nx][ny] && grid[nx][ny] == 0 {
                visited[nx][ny] = true
                queue = append(queue, [3]int{nx, ny, dist + 1})
            }
        }
    }

    return -1
}
```

### 🧠 Explanation:
- BFS ensures shortest path in **unweighted grid**.
- Explore all 8 directions from each point.

---

## 📚 Summary Table

| Problem                          | Graph Type        | Algorithm     | Notes                                      |
|----------------------------------|-------------------|---------------|--------------------------------------------|
| Network Delay Time               | Weighted + Directed | Dijkstra      | Use priority queue                         |
| Cheapest Flights Within K Stops | Weighted + Constraint | BFS + K limit | Track stops as layer/depth                 |
| Shortest Path in Binary Matrix  | Unweighted Grid   | BFS           | 8-direction traversal, shortest path       |

---
