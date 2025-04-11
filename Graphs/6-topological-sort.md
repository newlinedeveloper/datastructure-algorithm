Great! Let’s break down the **Topological Sort** pattern — one of the most **common patterns in interviews** at Google, Meta, etc., especially when problems involve **dependencies, scheduling, or ordering**.

---

## 🔁 6. Topological Sort

### 🔍 **How to Identify This Pattern**

Look for keywords like:
- "Order of tasks"
- "Task A before task B"
- "Can all tasks be finished?"
- "Build dependencies"
- "Alien language order"

You're dealing with a **Directed Acyclic Graph (DAG)** if:
- The graph is **directed**
- There’s an **implied order**
- You need to **check for cycles** (invalid schedule)

---

### 🧠 **How to Solve**

Two main approaches:

### ✅ **1. DFS + Post-Order (Reverse)**
- Perform DFS
- Append node after visiting all its neighbors (post-order)
- Reverse the result list

### ✅ **2. Kahn’s Algorithm (BFS + In-degree)**
- Count in-degrees of all nodes
- Push all 0 in-degree nodes into a queue
- Pop from queue, reduce in-degrees, and build result

---

## 🔹 Problem 1: Course Schedule II  
📌 [Leetcode 210](https://leetcode.com/problems/course-schedule-ii)

> Find a valid order to finish all courses given prerequisites.

### ✅ Input:
```go
numCourses = 4
prerequisites = [[1,0],[2,0],[3,1],[3,2]]
```

### ✅ Output:
```go
[0,1,2,3] or [0,2,1,3]
```

---

### 🚀 Go Code (Kahn's Algorithm):

```go
func findOrder(numCourses int, prerequisites [][]int) []int {
    graph := make(map[int][]int)
    inDegree := make([]int, numCourses)

    for _, pair := range prerequisites {
        dest, src := pair[0], pair[1]
        graph[src] = append(graph[src], dest)
        inDegree[dest]++
    }

    queue := []int{}
    for i := 0; i < numCourses; i++ {
        if inDegree[i] == 0 {
            queue = append(queue, i)
        }
    }

    var order []int
    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        order = append(order, node)

        for _, neighbor := range graph[node] {
            inDegree[neighbor]--
            if inDegree[neighbor] == 0 {
                queue = append(queue, neighbor)
            }
        }
    }

    if len(order) == numCourses {
        return order
    }
    return []int{}
}
```

### 🧠 Explanation:
- Build the graph and count incoming edges (dependencies).
- Use a queue to process all courses with no prerequisites.
- If all nodes are processed, return the order.

---

## 🔹 Problem 2: Alien Dictionary  
📌 [Leetcode 269 (Premium)](https://leetcode.com/problems/alien-dictionary)

> Given a sorted list of alien words, determine the alphabet order.

### ✅ Input:
```go
words = ["wrt","wrf","er","ett","rftt"]
```

### ✅ Output:
```
"wertf"
```

---

### 🚀 Go Code (Topological Sort with In-Degree):

```go
func alienOrder(words []string) string {
    graph := map[byte][]byte{}
    inDegree := map[byte]int{}
    
    // Initialize graph
    for _, word := range words {
        for i := 0; i < len(word); i++ {
            inDegree[word[i]] = 0
        }
    }

    // Build graph
    for i := 0; i < len(words)-1; i++ {
        w1, w2 := words[i], words[i+1]
        minLen := min(len(w1), len(w2))
        if len(w1) > len(w2) && w1[:minLen] == w2[:minLen] {
            return ""
        }
        for j := 0; j < minLen; j++ {
            if w1[j] != w2[j] {
                graph[w1[j]] = append(graph[w1[j]], w2[j])
                inDegree[w2[j]]++
                break
            }
        }
    }

    queue := []byte{}
    for ch, deg := range inDegree {
        if deg == 0 {
            queue = append(queue, ch)
        }
    }

    var result []byte
    for len(queue) > 0 {
        ch := queue[0]
        queue = queue[1:]
        result = append(result, ch)

        for _, nei := range graph[ch] {
            inDegree[nei]--
            if inDegree[nei] == 0 {
                queue = append(queue, nei)
            }
        }
    }

    if len(result) != len(inDegree) {
        return ""
    }

    return string(result)
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### 🧠 Explanation:
- Extract character order by comparing adjacent words.
- Build graph of character dependencies.
- Use topological sort to get valid character order.

---

## 🔹 Problem 3: Minimum Height Trees  
📌 [Leetcode 310](https://leetcode.com/problems/minimum-height-trees)

> Return roots of all Minimum Height Trees (MHTs).

### ✅ Input:
```go
n = 4, edges = [[1,0],[1,2],[1,3]]
```

### ✅ Output:
```
[1]
```

---

### 🚀 Go Code (Modified Topo Sort using Leaf Removal):

```go
func findMinHeightTrees(n int, edges [][]int) []int {
    if n == 1 {
        return []int{0}
    }

    graph := make(map[int][]int)
    degree := make([]int, n)

    for _, edge := range edges {
        u, v := edge[0], edge[1]
        graph[u] = append(graph[u], v)
        graph[v] = append(graph[v], u)
        degree[u]++
        degree[v]++
    }

    queue := []int{}
    for i := 0; i < n; i++ {
        if degree[i] == 1 {
            queue = append(queue, i)
        }
    }

    remaining := n
    for remaining > 2 {
        size := len(queue)
        remaining -= size
        for i := 0; i < size; i++ {
            node := queue[0]
            queue = queue[1:]
            for _, neighbor := range graph[node] {
                degree[neighbor]--
                if degree[neighbor] == 1 {
                    queue = append(queue, neighbor)
                }
            }
        }
    }

    return queue
}
```

### 🧠 Explanation:
- Remove leaves level by level until ≤ 2 nodes left.
- These are the root nodes that minimize tree height.

---

## 📚 Summary Table

| Problem                  | Pattern           | Approach              |
|--------------------------|-------------------|------------------------|
| Course Schedule II       | Topo Sort         | Kahn’s Algorithm       |
| Alien Dictionary         | Char Order (Topo) | DFS/BFS + In-degree    |
| Minimum Height Trees     | Leaf Pruning      | Topo-like layer prune  |

---
