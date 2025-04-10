Here are the **most important graph problem patterns**:

---

### 🔁 1. **Traversal Patterns**
**Goal:** Visit all nodes in a certain way.

- **Techniques**: BFS, DFS
- **Use Cases**:
  - Find connected components
  - Level-order traversal
  - Check if a graph is bipartite
- **Example**: [Number of Islands](https://leetcode.com/problems/number-of-islands/)

---

### 🔗 2. **Cycle Detection**
**Goal:** Detect if there's a cycle in the graph.

- **Techniques**:
  - **Undirected Graph**: Union-Find (Disjoint Set), DFS with parent tracking
  - **Directed Graph**: DFS with visited + recursion stack, Kahn's Algorithm (topo sort)
- **Use Cases**:
  - Detecting circular dependencies
- **Example**: [Course Schedule](https://leetcode.com/problems/course-schedule/)

---

### 📐 3. **Shortest Path**
**Goal:** Find the shortest path between nodes.

- **Techniques**:
  - **Unweighted**: BFS
  - **Weighted**: Dijkstra’s, Bellman-Ford, Floyd-Warshall, A*
- **Use Cases**:
  - GPS Navigation, Network routing
- **Example**: [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

---

### 🌊 4. **Flood Fill / Region Filling**
**Goal:** Explore and mark connected areas (like painting a region).

- **Techniques**: DFS or BFS
- **Use Cases**:
  - Maze solving, Image editing
- **Example**: [Flood Fill](https://leetcode.com/problems/flood-fill/)

---

### 🌐 5. **Connected Components**
**Goal:** Find separate groups of connected nodes.

- **Techniques**: DFS, BFS, Union-Find
- **Use Cases**:
  - Clustering, Finding isolated networks
- **Example**: [Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

---

### 🧱 6. **Topological Sort**
**Goal:** Order nodes such that all dependencies come before the dependent.

- **Techniques**: DFS post-order + reverse, Kahn's algorithm (BFS with in-degree)
- **Use Cases**:
  - Task scheduling, Build systems
- **Example**: [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

---

### 🌉 7. **Minimum Spanning Tree (MST)**
**Goal:** Connect all nodes with the minimum total edge weight.

- **Techniques**: Kruskal’s Algorithm, Prim’s Algorithm
- **Use Cases**:
  - Network design (cable, road)
- **Example**: [Connecting Cities With Minimum Cost](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)

---

### 🧩 8. **Union-Find (Disjoint Set Union - DSU)**
**Goal:** Track and merge connected components efficiently.

- **Techniques**: Union by rank, Path compression
- **Use Cases**:
  - Kruskal’s MST, Detecting cycles
- **Example**: [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

---

### 🧠 9. **Backtracking in Graphs**
**Goal:** Explore all paths or configurations with pruning.

- **Techniques**: DFS + recursion
- **Use Cases**:
  - Hamiltonian path/cycle, Word search in a grid
- **Example**: [Word Search](https://leetcode.com/problems/word-search/)

---

### 🧭 10. **Multi-source BFS / 0-1 BFS / Dijkstra Variants**
**Goal:** Solve problems with multiple starting points or special edge weights.

- **Techniques**: BFS with queue, Deque for 0-1 BFS, Min-heap for Dijkstra
- **Use Cases**:
  - Fire spreading, Rotting oranges, 0-1 weighted graphs
- **Example**: [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

----

## 🔁 1. Traversal Patterns (DFS/BFS)

### 🔍 **Identify When**:
- You need to **visit all nodes or regions**
- Problems involve **"counting islands", "exploring areas"**, or **traversing grids**

### 🧠 **Solve Using**:
- **DFS (recursive or stack)** for depth-first exploration  
- **BFS (queue)** for level-order or shortest steps in unweighted graphs  
- Use a **visited set/matrix** to avoid loops

### 📝 Practice:
- [Number of Islands](https://leetcode.com/problems/number-of-islands/)  
- [Clone Graph](https://leetcode.com/problems/clone-graph/)  
- [Max Area of Island](https://leetcode.com/problems/max-area-of-island/)

---

## 🔗 2. Cycle Detection

### 🔍 **Identify When**:
- You're asked: *Is there a loop/cycle?*
- In **dependency resolution**, **deadlock detection**, or **graph validity**

### 🧠 **Solve Using**:
- **Undirected**: DFS with parent or Union-Find  
- **Directed**: DFS + Recursion stack OR Kahn’s Algo (Topo Sort)

### 📝 Practice:
- [Course Schedule](https://leetcode.com/problems/course-schedule/)  
- [Redundant Connection](https://leetcode.com/problems/redundant-connection/)  
- [Detect Cycle in an Undirected Graph (GFG)](https://www.geeksforgeeks.org/detect-cycle-undirected-graph/)

---

## 📐 3. Shortest Path

### 🔍 **Identify When**:
- Asked for **shortest / minimum cost / fewest steps**
- Might mention **weights or constraints**

### 🧠 **Solve Using**:
- **BFS** for unweighted or equal-cost edges  
- **Dijkstra’s** for weighted graphs with non-negative weights  
- **Bellman-Ford** for graphs with negative weights

### 📝 Practice:
- [Network Delay Time](https://leetcode.com/problems/network-delay-time/)  
- [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)  
- [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/)

---

## 🌊 4. Flood Fill / Region Expansion

### 🔍 **Identify When**:
- You’re filling a **region** or area with certain criteria (like color or type)
- You need to **propagate or spread changes**

### 🧠 **Solve Using**:
- **DFS/BFS** with boundary conditions
- Modify original matrix or use a visited matrix

### 📝 Practice:
- [Flood Fill](https://leetcode.com/problems/flood-fill/)  
- [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)  
- [Walls and Gates](https://leetcode.com/problems/walls-and-gates/)

---

## 🌐 5. Connected Components

### 🔍 **Identify When**:
- Asked to **count or identify separate groups/subgraphs**
- Phrases like “provinces”, “groups”, “merge friends”

### 🧠 **Solve Using**:
- DFS/BFS traversal from every unvisited node  
- Union-Find to group nodes efficiently

### 📝 Practice:
- [Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)  
- [Accounts Merge](https://leetcode.com/problems/accounts-merge/)  
- [Count Sub Islands](https://leetcode.com/problems/count-sub-islands/)

---

## 🧱 6. Topological Sort

### 🔍 **Identify When**:
- You're given **dependencies (A before B)**  
- Asked to **order tasks** or check if they can be completed

### 🧠 **Solve Using**:
- **DFS + post-order + reverse**  
- **Kahn’s Algorithm** using queue and in-degrees

### 📝 Practice:
- [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)  
- [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)  
- [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

---

## 🌉 7. Minimum Spanning Tree (MST)

### 🔍 **Identify When**:
- You must **connect all nodes** at minimum cost  
- Phrases like “connect cities”, “lay wires/pipes”

### 🧠 **Solve Using**:
- **Kruskal’s** with Union-Find (greedy edge sorting)  
- **Prim’s** using a priority queue (grow from one node)

### 📝 Practice:
- [Connecting Cities With Minimum Cost](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)  
- [Optimize Water Distribution](https://leetcode.com/problems/optimize-water-distribution-in-a-village/)  
- [Minimum Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

---

## 🧩 8. Union-Find (Disjoint Set Union)

### 🔍 **Identify When**:
- You need to **track groups**, **merge components**, or **detect cycles**

### 🧠 **Solve Using**:
- DSU with **path compression** and **union by rank**  
- Very efficient for dynamic connectivity problems

### 📝 Practice:
- [Accounts Merge](https://leetcode.com/problems/accounts-merge/)  
- [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)  
- [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

---

## 🧠 9. Backtracking in Graphs

### 🔍 **Identify When**:
- You need to **explore all paths/configurations**  
- Typical in **mazes**, **permutations**, or **search puzzles**

### 🧠 **Solve Using**:
- DFS with **visited state tracking**  
- **Backtrack (undo move)** after recursive call

### 📝 Practice:
- [Word Search](https://leetcode.com/problems/word-search/)  
- [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)  
- [The Maze](https://leetcode.com/problems/the-maze/)

---

## 🧭 10. BFS Variants (0-1 BFS, Multi-source BFS)

### 🔍 **Identify When**:
- Shortest path with **special edge costs** (0/1)
- Multiple starting points, **spreading infection**, etc.

### 🧠 **Solve Using**:
- **Deque** for 0-1 BFS  
- **Multi-source BFS** by pushing all sources to queue

### 📝 Practice:
- [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)  
- [01 Matrix](https://leetcode.com/problems/01-matrix/)  
- [Shortest Path with Obstacles](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

---

### 📅 Suggested Weekly Practice Plan

| Week | Focus Pattern |
|------|----------------|
| Week 1 | Traversals & Flood Fill |
| Week 2 | Connected Components & Cycle Detection |
| Week 3 | Shortest Path & BFS Variants |
| Week 4 | Topological Sort & Backtracking |
| Week 5 | Union-Find & MST |

---

