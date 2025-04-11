Perfect! Let’s break down the **Cycle Detection** pattern in Graph DSA, which is critical for **Google interviews** and systems problems like **dependency resolution**, **deadlock prevention**, or **build order**.

---

## 🔁 2. Cycle Detection Pattern

### 🔍 How to Identify This Pattern

You’re likely dealing with a cycle detection problem if:
- You're asked:  
  ✅ "Can you finish all tasks?"  
  ✅ "Is there a loop in the graph?"  
  ✅ "Is the graph valid/acyclic?"  
  ✅ "Is there a redundant edge?"
- You’re working with:
  - **Directed graphs**: Topological sort / recursion stack for cycle
  - **Undirected graphs**: DFS with parent tracking or Union-Find

---

## 🔹 Problem 1: **Course Schedule**
📌 Leetcode: https://leetcode.com/problems/course-schedule/

### 🧾 Problem Summary:
Given `numCourses` and a list of prerequisite pairs, return `true` if you can finish all courses (i.e., the graph has **no cycle**).

---

### ✅ Sample Input:
```go
numCourses = 4
prerequisites = [[1,0],[2,1],[3,2]]
```

### ✅ Output:
```go
true
```

### 🚨 Cyclic Example:
```go
numCourses = 2
prerequisites = [[1,0],[0,1]]
```
**Output:** `false`

---

### 🚀 Go Code (DFS for Directed Graph):
```go
func canFinish(numCourses int, prerequisites [][]int) bool {
    graph := make(map[int][]int)
    for _, p := range prerequisites {
        graph[p[0]] = append(graph[p[0]], p[1])
    }

    visited := make([]int, numCourses) // 0 = unvisited, 1 = visiting, 2 = visited

    var hasCycle func(int) bool
    hasCycle = func(course int) bool {
        if visited[course] == 1 {
            return true // cycle detected
        }
        if visited[course] == 2 {
            return false
        }

        visited[course] = 1 // mark as visiting
        for _, prereq := range graph[course] {
            if hasCycle(prereq) {
                return true
            }
        }

        visited[course] = 2 // mark as done
        return false
    }

    for i := 0; i < numCourses; i++ {
        if hasCycle(i) {
            return false
        }
    }

    return true
}
```

### 🧠 Explanation:
- Use a **DFS traversal with state** tracking.
- `visiting (1)` → visiting in current path.
- If we reach a `visiting` node again → **cycle exists**.

---

## 🔹 Problem 2: **Redundant Connection**
📌 Leetcode: https://leetcode.com/problems/redundant-connection/

### 🧾 Problem Summary:
You're given a **tree + 1 extra edge** (i.e., graph with one cycle). Return the redundant edge that causes the cycle.

---

### ✅ Sample Input:
```go
edges = [[1,2],[1,3],[2,3]]
```

### ✅ Output:
```go
[2,3]
```

---

### 🚀 Go Code (Union-Find for Undirected Graph):
```go
func findRedundantConnection(edges [][]int) []int {
    parent := make([]int, 1001)
    for i := range parent {
        parent[i] = i
    }

    var find func(int) int
    find = func(x int) int {
        if parent[x] != x {
            parent[x] = find(parent[x]) // path compression
        }
        return parent[x]
    }

    var union func(int, int) bool
    union = func(x, y int) bool {
        rootX := find(x)
        rootY := find(y)
        if rootX == rootY {
            return false
        }
        parent[rootX] = rootY
        return true
    }

    for _, edge := range edges {
        if !union(edge[0], edge[1]) {
            return edge // this edge causes a cycle
        }
    }

    return []int{}
}
```

### 🧠 Explanation:
- Classic **Union-Find (Disjoint Set)** cycle detection.
- If `u` and `v` already have the same root → cycle detected → return the edge.

---

## 🔹 Problem 3: **Detect Cycle in an Undirected Graph**
📌 GFG: https://practice.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/

### 🧾 Problem Summary:
Given an undirected graph with `V` vertices and `E` edges, check if there is a **cycle**.

---

### ✅ Sample Input:
```txt
V = 5, Edges = [[0,1],[1,2],[2,0]]
```

### ✅ Output:
```txt
true
```

---

### 🚀 Go Code (DFS with Parent):
```go
func isCycle(V int, adj [][]int) bool {
    visited := make([]bool, V)

    var dfs func(int, int) bool
    dfs = func(node, parent int) bool {
        visited[node] = true
        for _, neighbor := range adj[node] {
            if !visited[neighbor] {
                if dfs(neighbor, node) {
                    return true
                }
            } else if neighbor != parent {
                return true // found a back edge
            }
        }
        return false
    }

    for i := 0; i < V; i++ {
        if !visited[i] {
            if dfs(i, -1) {
                return true
            }
        }
    }

    return false
}
```

---

### 🧠 Explanation:
- Standard DFS but track the **parent node**.
- If we revisit a visited node that's **not the parent**, a **cycle exists**.

---

## 🔚 Summary Table

| Problem                      | Graph Type  | Method         | Cycle Detection Logic                         |
|-----------------------------|-------------|----------------|-----------------------------------------------|
| Course Schedule             | Directed    | DFS + State    | Use `visited` (0=unvisited, 1=visiting, 2=done) |
| Redundant Connection        | Undirected  | Union-Find     | Return edge when two nodes already connected  |
| Detect Cycle (GFG)          | Undirected  | DFS + Parent   | Return true if visited neighbor ≠ parent      |

---

