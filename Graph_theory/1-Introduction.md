Sure! Let's dive into the **Introduction to Graph Theory - Part 1**:  
### ✅ **Overview and Representations of Graphs**

---

## 🔹 What is a Graph?
A **graph** is a data structure used to model **pairwise relationships** between objects.

- A graph `G` consists of:
  - A **set of vertices** (also called **nodes**), `V`
  - A **set of edges** `E`, where each edge connects a pair of vertices

---

## 🔹 Types of Graphs:

1. **Undirected Graph**:
   - Edges do not have direction: (u, v) = (v, u)
   - Example: Friendship network

2. **Directed Graph (Digraph)**:
   - Edges have direction: (u → v) ≠ (v → u)
   - Example: Twitter follower relationships

3. **Weighted Graph**:
   - Each edge has a weight/cost (e.g., distance, time, etc.)

4. **Unweighted Graph**:
   - All edges are equal in cost/weight

5. **Cyclic vs Acyclic Graphs**:
   - **Cyclic**: Graph contains at least one cycle
   - **Acyclic**: No cycles

6. **Connected Graph** (Undirected):
   - There's a path between every pair of vertices

7. **Strongly Connected Graph** (Directed):
   - There's a path from every node to every other node

---

## 🔹 Graph Representation in Code:

### 1. **Adjacency Matrix**  
- A 2D array of size `n x n`  
- `matrix[i][j] = 1` if there is an edge from node `i` to `j`

**Pros**:
- Easy to implement
- Faster lookups: O(1)

**Cons**:
- Space complexity: O(n²)
- Not efficient for sparse graphs

```go
// Go: Adjacency Matrix (undirected)
n := 5
graph := make([][]int, n)
for i := range graph {
	graph[i] = make([]int, n)
}
graph[0][1] = 1
graph[1][0] = 1 // because undirected
```

---

### 2. **Adjacency List**  
- Each node stores a list of its neighbors

**Pros**:
- Efficient space: O(n + e)
- Fast to iterate over neighbors

**Cons**:
- Slightly slower edge checks: O(k) where k is degree of node

```go
// Go: Adjacency List
graph := make(map[int][]int)
graph[0] = append(graph[0], 1)
graph[1] = append(graph[1], 0)
```

---

### 3. **Edge List**
- A list of all edges: `[(u1, v1), (u2, v2), ...]`
- Simple to store, used in Kruskal’s algorithm

```go
edges := [][]int{
	{0, 1},
	{1, 2},
	{2, 3},
}
```

---

## 🔹 When to Use What?
| Representation     | Best For                    | Lookup     | Space      |
|--------------------|-----------------------------|------------|------------|
| Adjacency Matrix   | Dense graphs                | O(1)       | O(n²)      |
| Adjacency List     | Sparse graphs, DFS/BFS      | O(k)       | O(n + e)   |
| Edge List          | Sorting edges (Kruskal’s)   | O(n)       | O(e)       |

---

## 🔹 Real-Life Examples:
- **Google Maps**: Cities = nodes, roads = edges
- **Social Networks**: Users = nodes, connections/follows = edges
- **Internet**: Routers = nodes, links = edges

---
