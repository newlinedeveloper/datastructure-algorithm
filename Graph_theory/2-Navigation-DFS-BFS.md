Absolutely! Let's dive into:

---

# 🔍 Graph Theory Introduction – **Part 2**  
## 🧭 DFS and BFS: Graph Navigation

---

## 🔸 What is Graph Navigation?

Graph navigation refers to **visiting/traversing nodes** in a graph — useful for finding:
- All reachable nodes
- Connected components
- Shortest paths
- Cycles
- etc.

---

## 🔹 Two Fundamental Graph Traversal Algorithms

### 1. **DFS (Depth-First Search)**

#### 🔧 Idea:
Explore **as far as possible along one path**, then backtrack.

#### ✅ Characteristics:
- Uses **recursion** or a **stack**
- Good for **exploring entire connected components**
- Helps in:
  - Detecting cycles
  - Topological sorting
  - Connected components

#### 💡 Example (recursive DFS in Go):

```go
func dfs(node int, graph map[int][]int, visited map[int]bool) {
    if visited[node] {
        return
    }
    visited[node] = true
    fmt.Printf("%d ", node)
    for _, neighbor := range graph[node] {
        dfs(neighbor, graph, visited)
    }
}
```

---

### 2. **BFS (Breadth-First Search)**

#### 🔧 Idea:
Explore all **neighbors first**, then their neighbors.

#### ✅ Characteristics:
- Uses a **queue**
- Finds the **shortest path in unweighted graphs**
- Level-order traversal (like in trees)

#### 💡 Example (BFS in Go):

```go
func bfs(start int, graph map[int][]int) {
    visited := make(map[int]bool)
    queue := []int{start}
    visited[start] = true

    for len(queue) > 0 {
        current := queue[0]
        queue = queue[1:]
        fmt.Printf("%d ", current)

        for _, neighbor := range graph[current] {
            if !visited[neighbor] {
                visited[neighbor] = true
                queue = append(queue, neighbor)
            }
        }
    }
}
```

---

## 🔸 Example Graph

```
0 -- 1 -- 2
|         |
3 ------- 4
```

**Adjacency List in Go:**

```go
graph := map[int][]int{
    0: {1, 3},
    1: {0, 2},
    2: {1, 4},
    3: {0, 4},
    4: {2, 3},
}
```

---

## 🔹 DFS vs BFS Comparison

| Feature              | DFS                   | BFS                   |
|----------------------|------------------------|------------------------|
| Data structure       | Stack / Recursion      | Queue                 |
| Space complexity     | O(V)                   | O(V)                  |
| Finds shortest path? | ❌ (unless all edges = 1) | ✅                   |
| Application          | Cycle detection, Topo sort | Shortest path, Level order |
| Explores             | Deepest path first     | Closest nodes first   |

---

## 🛠 Use Cases

| Problem                            | BFS / DFS?          |
|-----------------------------------|----------------------|
| Shortest path in unweighted graph | ✅ BFS               |
| Detecting cycles in graph         | ✅ DFS               |
| Number of connected components    | ✅ DFS/BFS           |
| Level order traversal of tree     | ✅ BFS               |
| Maze solving                      | ✅ BFS (shortest), ✅ DFS (any path) |

---

Would you like to proceed with:
- Part 3: **Graph Applications** (Connected Components, Cycle Detection, etc.), or  
- Example Problems like **Counting Rooms**, **Bipartite Check**, or **Topological Sort**?
