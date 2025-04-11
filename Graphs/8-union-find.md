Let’s dive into the **Union-Find (Disjoint Set Union)** pattern — a key technique in many graph problems, especially those involving grouping, merging, and cycle detection.

---

## 💡 Pattern 8: Union-Find (Disjoint Set Union)

### 🔍 How to Identify This Pattern:
Look for keywords like:
- “Merge”, “Union”, “Group”
- “Belong to the same group”
- “Number of connected components”
- “Detect if adding an edge forms a cycle”

If the problem involves **checking or merging components dynamically**, it's likely a **Union-Find** problem.

---

## 🧠 Core Idea:
Maintain a data structure that:
- Tracks the **parent** (representative) of each group
- Supports efficient **Find** and **Union** operations
- Optimized with:
  - **Path Compression** during `find()`
  - **Union by Rank/Size** to keep trees shallow

---

## 🧪 Problem 1: **Accounts Merge**  
📌 LeetCode 721

### ✅ Problem Summary:
You're given a list of accounts where each account contains a name and a list of emails. Merge accounts that belong to the same person (emails in common).

---

### 🧠 Approach:
- Use Union-Find to connect emails
- Map each email to an account owner
- Union emails belonging to the same person
- After unions, group emails by root parent

---

### ✅ Go Code:

```go
import (
    "sort"
)

type UnionFind struct {
    parent map[string]string
}

func NewUF() *UnionFind {
    return &UnionFind{parent: map[string]string{}}
}

func (uf *UnionFind) Find(x string) string {
    if uf.parent[x] != x {
        uf.parent[x] = uf.Find(uf.parent[x])
    }
    return uf.parent[x]
}

func (uf *UnionFind) Union(x, y string) {
    rootX := uf.Find(x)
    rootY := uf.Find(y)
    if rootX != rootY {
        uf.parent[rootY] = rootX
    }
}

func accountsMerge(accounts [][]string) [][]string {
    uf := NewUF()
    emailToName := map[string]string{}

    for _, account := range accounts {
        name := account[0]
        firstEmail := account[1]
        for _, email := range account[1:] {
            if _, exists := uf.parent[email]; !exists {
                uf.parent[email] = email
            }
            emailToName[email] = name
            uf.Union(firstEmail, email)
        }
    }

    rootToEmails := map[string][]string{}
    for email := range uf.parent {
        root := uf.Find(email)
        rootToEmails[root] = append(rootToEmails[root], email)
    }

    var result [][]string
    for root, emails := range rootToEmails {
        sort.Strings(emails)
        name := emailToName[root]
        result = append(result, append([]string{name}, emails...))
    }
    return result
}
```

---

### ✅ Input:
```go
accounts := [][]string{
    {"John", "johnsmith@mail.com", "john00@mail.com"},
    {"John", "johnnybravo@mail.com"},
    {"John", "johnsmith@mail.com", "john_newyork@mail.com"},
    {"Mary", "mary@mail.com"},
}
```

### ✅ Output:
```
[
  ["John", "john00@mail.com", "john_newyork@mail.com", "johnsmith@mail.com"],
  ["John", "johnnybravo@mail.com"],
  ["Mary", "mary@mail.com"]
]
```

---

## 🧪 Problem 2: **Number of Provinces**  
📌 LeetCode 547

### ✅ Problem Summary:
Given a matrix `isConnected[i][j]` representing connections between cities, return the number of provinces (connected components).

---

### ✅ Go Code:

```go
func findCircleNum(isConnected [][]int) int {
    n := len(isConnected)
    parent := make([]int, n)
    for i := 0; i < n; i++ {
        parent[i] = i
    }

    var find func(int) int
    find = func(x int) int {
        if parent[x] != x {
            parent[x] = find(parent[x])
        }
        return parent[x]
    }

    union := func(x, y int) {
        rootX := find(x)
        rootY := find(y)
        if rootX != rootY {
            parent[rootY] = rootX
        }
    }

    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            if isConnected[i][j] == 1 {
                union(i, j)
            }
        }
    }

    count := 0
    for i := 0; i < n; i++ {
        if find(i) == i {
            count++
        }
    }
    return count
}
```

---

## 🧪 Problem 3: **Redundant Connection**  
📌 LeetCode 684

### ✅ Problem Summary:
You're given a graph that started as a tree with one extra edge. Find the edge that creates a cycle.

---

### ✅ Go Code:

```go
func findRedundantConnection(edges [][]int) []int {
    parent := make([]int, 1001)
    for i := range parent {
        parent[i] = i
    }

    var find func(int) int
    find = func(x int) int {
        if parent[x] != x {
            parent[x] = find(parent[x])
        }
        return parent[x]
    }

    for _, edge := range edges {
        u, v := edge[0], edge[1]
        pu, pv := find(u), find(v)
        if pu == pv {
            return edge // cycle found
        }
        parent[pu] = pv
    }

    return nil
}
```

---

## ✅ Summary:

| Problem                    | Identify By                                    | Union-Find Used For            |
|----------------------------|------------------------------------------------|--------------------------------|
| Accounts Merge             | Merging emails                                 | Grouping same emails           |
| Number of Provinces        | Count of connected components                  | Grouping cities                |
| Redundant Connection       | Find edge forming a cycle                      | Cycle detection                |

---
