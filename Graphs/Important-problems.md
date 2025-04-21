### 🧠 Common Graph Problems in  Interviews

1. **Clone Graph**
   - **Problem Type**: Graph Traversal (DFS/BFS)
   - **Description**:Given a reference of a node in a connected undirected graph, return a deep copy (clone) of the graph

2. **Course Schedule**
   - **Problem Type**: Topological Sort
   - **Description**:Determine if it's possible to finish all courses given the prerequisites

3. **Evaluate Division**
   - **Problem Type**: Graph with Weighted Edges
   - **Description**:Given equations like A / B = k, evaluate queries to find the result of division between variables

4. **Number of Islands**
   - **Problem Type**: DFS/BFS on Grid
   - **Description**:Count the number of islands in a 2D grid map

5. **Is Graph Bipartite?**
   - **Problem Type**: Graph Coloring
   - **Description**:Determine if a graph can be colored using two colors without adjacent nodes having the same color

6. **Redundant Connection**
   - **Problem Type**: Union-Find (Disjoint Set Union)
   - **Description**:In a tree with one extra edge added, find the edge that can be removed to restore the tree

7. **Alien Dictionary**
   - **Problem Type**: Topological Sort
   - **Description**:Given a sorted dictionary of an alien language, find the order of characters

8. **Minimum Height Trees**
   - **Problem Type**: Tree Center Finding
   - **Description**:Find all the roots of Minimum Height Trees in a given undirected graph

9. **Reconstruct Itinerary**
   - **Problem Type**: Eulerian Path
   - **Description**:Reconstruct the itinerary from a list of airline tickets

10. **Word Ladder**
    - **Problem Type**: BFS
    - **Description**: Find the shortest transformation sequence from start word to end word, changing only one letter at a time.

---


## 🧠 1. Clone Graph

- **Problem Type:** Graph Traversal (DFS or BFS)
- **Pattern:** Graph traversal + visited map
- **Approach:**
  1. Use DFS or BFS.
  2. Use a `map[*Node]*Node` to track already cloned nodes.
  3. Traverse neighbors, clone them recursively.

```go
func cloneGraph(node *Node) *Node {
	if node == nil {
		return nil
	}
	visited := map[*Node]*Node{}
	var clone func(*Node) *Node
	clone = func(n *Node) *Node {
		if n == nil {
			return nil
		}
		if _, ok := visited[n]; ok {
			return visited[n]
		}
		copy := &Node{Val: n.Val}
		visited[n] = copy
		for _, neighbor := range n.Neighbors {
			copy.Neighbors = append(copy.Neighbors, clone(neighbor))
		}
		return copy
	}
	return clone(node)
}
```

---

## 📚 2. Course Schedule

- **Problem Type:** Topological Sort
- **Pattern:** Cycle Detection in Directed Graph
- **Approach:**
  1. Build graph from prerequisites.
  2. Do DFS; check if there's a cycle using `visited` and `recStack`.
  3. Alternatively, use **Kahn’s algorithm** (BFS with indegree count).

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
	graph := make([][]int, numCourses)
	inDegree := make([]int, numCourses)

	for _, p := range prerequisites {
		graph[p[1]] = append(graph[p[1]], p[0])
		inDegree[p[0]]++
	}

	queue := []int{}
	for i := 0; i < numCourses; i++ {
		if inDegree[i] == 0 {
			queue = append(queue, i)
		}
	}

	count := 0
	for len(queue) > 0 {
		curr := queue[0]
		queue = queue[1:]
		count++
		for _, next := range graph[curr] {
			inDegree[next]--
			if inDegree[next] == 0 {
				queue = append(queue, next)
			}
		}
	}

	return count == numCourses
}
```

---

## ➗ 3. Evaluate Division

- **Problem Type:** Graph with Weighted Edges
- **Pattern:** Graph construction + DFS or Union-Find
- **Approach:**
  1. Build graph where each node connects to another with a weight.
  2. Use DFS from numerator to denominator, multiplying weights.

```go
func calcEquation(equations [][]string, values []float64, queries [][]string) []float64 {
	graph := map[string][][2]interface{}{}

	for i, eq := range equations {
		a, b := eq[0], eq[1]
		val := values[i]
		graph[a] = append(graph[a], [2]interface{}{b, val})
		graph[b] = append(graph[b], [2]interface{}{a, 1 / val})
	}

	var dfs func(a, b string, visited map[string]bool) float64
	dfs = func(a, b string, visited map[string]bool) float64 {
		if _, ok := graph[a]; !ok {
			return -1
		}
		if a == b {
			return 1
		}
		visited[a] = true
		for _, pair := range graph[a] {
			neighbor, weight := pair[0].(string), pair[1].(float64)
			if visited[neighbor] {
				continue
			}
			product := dfs(neighbor, b, visited)
			if product != -1 {
				return weight * product
			}
		}
		return -1
	}

	res := make([]float64, len(queries))
	for i, q := range queries {
		res[i] = dfs(q[0], q[1], map[string]bool{})
	}
	return res
}
```

---

## 🌊 4. Number of Islands

- **Problem Type:** DFS/BFS on Grid
- **Pattern:** Connected Component Count
- **Approach:**
  1. Traverse grid.
  2. On seeing `'1'`, run DFS/BFS to mark all connected land.
  3. Count how many times you do this.

```go
func numIslands(grid [][]byte) int {
	if len(grid) == 0 {
		return 0
	}
	rows, cols := len(grid), len(grid[0])
	var dfs func(int, int)

	dfs = func(r, c int) {
		if r < 0 || c < 0 || r >= rows || c >= cols || grid[r][c] != '1' {
			return
		}
		grid[r][c] = '0'
		dfs(r+1, c)
		dfs(r-1, c)
		dfs(r, c+1)
		dfs(r, c-1)
	}

	count := 0
	for r := 0; r < rows; r++ {
		for c := 0; c < cols; c++ {
			if grid[r][c] == '1' {
				dfs(r, c)
				count++
			}
		}
	}
	return count
}
```

---

## 🎨 5. Is Graph Bipartite?

- **Problem Type:** Graph Coloring
- **Pattern:** BFS/DFS + Coloring
- **Approach:**
  1. Assign two colors alternately to adjacent nodes.
  2. If a node gets the same color as its neighbor → Not bipartite.

```go
func isBipartite(graph [][]int) bool {
	colors := make([]int, len(graph))
	for i := range colors {
		colors[i] = -1
	}

	var dfs func(int, int) bool
	dfs = func(node, color int) bool {
		if colors[node] != -1 {
			return colors[node] == color
		}
		colors[node] = color
		for _, neighbor := range graph[node] {
			if !dfs(neighbor, 1-color) {
				return false
			}
		}
		return true
	}

	for i := range graph {
		if colors[i] == -1 && !dfs(i, 0) {
			return false
		}
	}
	return true
}
```

---

## 🔗 6. Redundant Connection

- **Problem Type:** Union-Find
- **Pattern:** Detect Cycle in Undirected Graph
- **Approach:**
  1. Initialize Union-Find structure.
  2. For each edge, try to union.
  3. If union fails (already connected), it's the redundant edge.

```go
func findRedundantConnection(edges [][]int) []int {
	parent := make([]int, 1001)
	for i := 1; i <= 1000; i++ {
		parent[i] = i
	}

	var find func(int) int
	find = func(x int) int {
		if parent[x] != x {
			parent[x] = find(parent[x])
		}
		return parent[x]
	}

	union := func(x, y int) bool {
		px, py := find(x), find(y)
		if px == py {
			return false
		}
		parent[py] = px
		return true
	}

	for _, edge := range edges {
		if !union(edge[0], edge[1]) {
			return edge
		}
	}
	return nil
}
```

---
Here are **solutions with explanations** and common patterns for the remaining problems you mentioned:

---

### 🔤 **Alien Dictionary**

- **Pattern**: Topological Sort (Graph)
- **Problem**: Given a sorted dictionary (list of words) from an alien language, derive the order of characters.
  
#### ✅ Approach:
1. Build a **graph** of characters and **indegree** array.
2. For each pair of adjacent words, compare characters to find the **first different** character and create a **directed edge**.
3. Perform **Kahn's Algorithm** (BFS-based topological sort) to determine order.

#### ✅ Go Code:
```
func alienOrder(words []string) string {
	graph := make(map[byte][]byte)
	inDegree := make(map[byte]int)

	for _, word := range words {
		for i := 0; i < len(word); i++ {
			inDegree[word[i]] = 0
		}
	}

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

	var res []byte
	for len(queue) > 0 {
		ch := queue[0]
		queue = queue[1:]
		res = append(res, ch)
		for _, nei := range graph[ch] {
			inDegree[nei]--
			if inDegree[nei] == 0 {
				queue = append(queue, nei)
			}
		}
	}

	if len(res) != len(inDegree) {
		return ""
	}
	return string(res)
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

```

---

### 🌳 **Minimum Height Trees**

- **Pattern**: Tree Center Finding
- **Problem**: Given an undirected tree with `n` nodes, return the **roots** of Minimum Height Trees (MHTs).

#### ✅ Approach:
1. Tree centers are found by **peeling leaves** layer by layer until 1 or 2 nodes are left.
2. Use a **graph with degree tracking**, remove leaves in each round.

#### ✅ Go Code:
```
func findMinHeightTrees(n int, edges [][]int) []int {
	if n == 1 {
		return []int{0}
	}

	graph := make([][]int, n)
	degree := make([]int, n)

	for _, e := range edges {
		u, v := e[0], e[1]
		graph[u] = append(graph[u], v)
		graph[v] = append(graph[v], u)
		degree[u]++
		degree[v]++
	}

	leaves := []int{}
	for i := 0; i < n; i++ {
		if degree[i] == 1 {
			leaves = append(leaves, i)
		}
	}

	for n > 2 {
		n -= len(leaves)
		newLeaves := []int{}
		for _, leaf := range leaves {
			for _, neighbor := range graph[leaf] {
				degree[neighbor]--
				if degree[neighbor] == 1 {
					newLeaves = append(newLeaves, neighbor)
				}
			}
		}
		leaves = newLeaves
	}

	return leaves
}

```

---

### ✈️ **Reconstruct Itinerary**

- **Pattern**: Hierholzer’s Algorithm (Eulerian Path)
- **Problem**: Reconstruct an itinerary from a list of airline tickets such that the itinerary uses all tickets once and is lexicographically smallest.

#### ✅ Approach:
1. Build an **adjacency list** using a **min-heap** to ensure lexical order.
2. Use **DFS** to build the path and append to result in **postorder**.

#### ✅ Golang Code:
```golang
import (
	"container/heap"
	"sort"
)

type MinHeap []string

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(string))
}
func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[:n-1]
	return x
}

func findItinerary(tickets [][]string) []string {
	graph := make(map[string]*MinHeap)

	for _, ticket := range tickets {
		src, dst := ticket[0], ticket[1]
		if _, exists := graph[src]; !exists {
			graph[src] = &MinHeap{}
		}
		heap.Push(graph[src], dst)
	}

	var res []string
	var dfs func(string)
	dfs = func(airport string) {
		for graph[airport] != nil && graph[airport].Len() > 0 {
			next := heap.Pop(graph[airport]).(string)
			dfs(next)
		}
		res = append(res, airport)
	}

	dfs("JFK")
	// Reverse result
	for i, j := 0, len(res)-1; i < j; i, j = i+1, j-1 {
		res[i], res[j] = res[j], res[i]
	}
	return res
}

```

---

### 🪜 **Word Ladder**

- **Pattern**: BFS (Shortest Path in Unweighted Graph)
- **Problem**: Find the length of shortest transformation sequence from `beginWord` to `endWord`, changing one letter at a time and using only words from `wordList`.

#### ✅ Approach:
1. Use **BFS** to explore all one-letter transformations.
2. Generate all neighbors by replacing each character with 'a' to 'z'.
3. Track visited words to avoid cycles.

#### ✅ Golang Code:
```golang
import "container/list"

func ladderLength(beginWord string, endWord string, wordList []string) int {
	wordSet := make(map[string]bool)
	for _, word := range wordList {
		wordSet[word] = true
	}
	if !wordSet[endWord] {
		return 0
	}

	queue := list.New()
	queue.PushBack([2]interface{}{beginWord, 1})

	for queue.Len() > 0 {
		curr := queue.Remove(queue.Front()).([2]interface{})
		word := curr[0].(string)
		steps := curr[1].(int)

		if word == endWord {
			return steps
		}

		for i := 0; i < len(word); i++ {
			for c := 'a'; c <= 'z'; c++ {
				newWord := word[:i] + string(c) + word[i+1:]
				if wordSet[newWord] {
					queue.PushBack([2]interface{}{newWord, steps + 1})
					delete(wordSet, newWord)
				}
			}
		}
	}

	return 0
}

```

---



