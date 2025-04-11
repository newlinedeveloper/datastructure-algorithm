Great! Let's dive into the **Connected Components** graph pattern — a favorite in tech interviews like Google, especially when dealing with relationships, subgraphs, and clustering.

---

## 🔁 5. Connected Components Pattern

### 🔍 **How to Identify This Pattern**

You'll usually see phrases like:
- “How many groups…”
- “Are these nodes connected?”
- “Count number of islands/provinces/groups”
- “Merge accounts/friends/networks”

Common in:
- Unlabeled graphs or grids
- Problems requiring you to find **distinct connected groups**

---

### 🧠 **How to Solve**

Two main strategies:

#### 1. **DFS/BFS**
- Start traversal from each unvisited node.
- Mark all connected nodes as visited.
- Count how many times you have to start a new traversal.

#### 2. **Union-Find (Disjoint Set Union – DSU)**
- Great for dynamic connectivity and merging sets.
- Use for problems involving grouping via equivalency or merging.

---

## 🔹 Problem 1: Number of Connected Components in an Undirected Graph  
📌 Leetcode [#323](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph)

### ✅ Sample Input:
```go
n = 5
edges = [[0,1],[1,2],[3,4]]
```

### ✅ Output:
```
2  // Components: [0-1-2], [3-4]
```

---

### 🚀 Go Code (DFS):

```go
func countComponents(n int, edges [][]int) int {
    graph := make(map[int][]int)
    for _, edge := range edges {
        u, v := edge[0], edge[1]
        graph[u] = append(graph[u], v)
        graph[v] = append(graph[v], u)
    }

    visited := make([]bool, n)
    count := 0

    var dfs func(node int)
    dfs = func(node int) {
        visited[node] = true
        for _, neighbor := range graph[node] {
            if !visited[neighbor] {
                dfs(neighbor)
            }
        }
    }

    for i := 0; i < n; i++ {
        if !visited[i] {
            dfs(i)
            count++
        }
    }

    return count
}
```

### 🧠 Explanation:
- Build adjacency list
- DFS from each unvisited node
- Every new DFS call indicates a new component

---

## 🔹 Problem 2: Accounts Merge  
📌 Leetcode [#721](https://leetcode.com/problems/accounts-merge)

Merge accounts where any email overlaps. Each account looks like: `["Name", email1, email2, ...]`.

### ✅ Sample Input:
```go
[
  ["John", "john1@mail.com", "john2@mail.com"],
  ["John", "john2@mail.com", "john3@mail.com"],
  ["Mary", "mary@mail.com"]
]
```

### ✅ Output:
```go
[
  ["John", "john1@mail.com", "john2@mail.com", "john3@mail.com"],
  ["Mary", "mary@mail.com"]
]
```

---

### 🚀 Go Code (Union-Find):

```go
func accountsMerge(accounts [][]string) [][]string {
    parent := map[string]string{}
    emailToName := map[string]string{}

    var find func(string) string
    find = func(x string) string {
        if parent[x] != x {
            parent[x] = find(parent[x])
        }
        return parent[x]
    }

    union := func(x, y string) {
        parent[find(x)] = find(y)
    }

    for _, acc := range accounts {
        name := acc[0]
        for _, email := range acc[1:] {
            if _, exists := parent[email]; !exists {
                parent[email] = email
            }
            emailToName[email] = name
            union(acc[1], email) // union first email with current
        }
    }

    groups := map[string][]string{}
    for email := range parent {
        root := find(email)
        groups[root] = append(groups[root], email)
    }

    res := [][]string{}
    for root, emails := range groups {
        sort.Strings(emails)
        res = append(res, append([]string{emailToName[root]}, emails...))
    }

    return res
}
```

### 🧠 Explanation:
- Use Union-Find to group emails that belong together.
- Map email to name and group by root email.
- Merge and sort for final output.

---

## 🔹 Problem 3: Count Sub Islands  
📌 Leetcode [#1905](https://leetcode.com/problems/count-sub-islands)

Count the number of islands in `grid2` that are **completely inside** islands in `grid1`.

### ✅ Sample Input:
```go
grid1 = [
  [1,1,1,0],
  [0,1,1,1],
  [0,0,0,1]
]
grid2 = [
  [1,1,1,0],
  [0,0,1,1],
  [0,1,0,1]
]
```

### ✅ Output:
```
2
```

---

### 🚀 Go Code (DFS):

```go
func countSubIslands(grid1 [][]int, grid2 [][]int) int {
    rows, cols := len(grid1), len(grid1[0])

    var dfs func(r, c int) bool
    dfs = func(r, c int) bool {
        if r < 0 || r >= rows || c < 0 || c >= cols || grid2[r][c] == 0 {
            return true
        }

        valid := grid1[r][c] == 1
        grid2[r][c] = 0 // visited

        dirs := [][2]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
        for _, d := range dirs {
            valid = dfs(r+d[0], c+d[1]) && valid
        }

        return valid
    }

    count := 0
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if grid2[r][c] == 1 {
                if dfs(r, c) {
                    count++
                }
            }
        }
    }

    return count
}
```

### 🧠 Explanation:
- DFS to check if every cell in a `grid2` island is also `1` in `grid1`.
- If all match, it's a sub-island.

---

## 📚 Summary Table

| Problem                   | Strategy   | Key Insight                            |
|---------------------------|------------|----------------------------------------|
| Number of Components      | DFS/BFS    | Count unvisited DFS calls              |
| Accounts Merge            | Union-Find | Group by equivalence using DSU         |
| Count Sub Islands         | DFS        | Check sub-island constraint recursively|

---
