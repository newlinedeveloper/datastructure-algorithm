### Graph

#### Number of Islands

```
package main

import "fmt"

func numIslands(grid [][]byte) int {
	if len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}

	m, n := len(grid), len(grid[0])
	count := 0

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == '1' {
				count++
				dfs(grid, i, j)
			}
		}
	}

	return count
}

func dfs(grid [][]byte, row, col int) {
	m, n := len(grid), len(grid[0])

	if row < 0 || row >= m || col < 0 || col >= n || grid[row][col] != '1' {
		return
	}

	grid[row][col] = '0'
	dfs(grid, row+1, col)
	dfs(grid, row-1, col)
	dfs(grid, row, col+1)
	dfs(grid, row, col-1)
}

func main() {
	grid := [][]byte{
		{'1', '1', '1', '1', '0'},
		{'1', '1', '0', '1', '0'},
		{'1', '1', '0', '0', '0'},
		{'0', '0', '0', '0', '0'},
	}

	result := numIslands(grid)
	fmt.Println(result)
}

```
In this code, the numIslands function takes a 2D grid representing a map as input and returns the number of islands present in the grid.

The dfs function is a helper function that performs depth-first search (DFS) to explore the connected land cells. It takes the grid, current row and column indices as parameters. It marks the current cell as visited by changing its value to '0', and then recursively explores its neighboring cells (up, down, left, right) to find other connected land cells. By doing so, it effectively explores and counts the number of islands in the grid.

In the main function, we define a 2D grid and call the numIslands function to solve the problem. The result is printed to the console.

#### Clone Graph

```
package main

import "fmt"

// Definition for a Node.
type Node struct {
	Val       int
	Neighbors []*Node
}

func cloneGraph(node *Node) *Node {
	if node == nil {
		return nil
	}

	visited := make(map[*Node]*Node)
	return dfs(node, visited)
}

func dfs(node *Node, visited map[*Node]*Node) *Node {
	// If the node has already been visited, return the cloned node
	if _, ok := visited[node]; ok {
		return visited[node]
	}

	// Create a clone of the current node
	cloneNode := &Node{Val: node.Val}
	visited[node] = cloneNode

	// Clone the neighbors of the current node recursively
	for _, neighbor := range node.Neighbors {
		cloneNeighbor := dfs(neighbor, visited)
		cloneNode.Neighbors = append(cloneNode.Neighbors, cloneNeighbor)
	}

	return cloneNode
}

func main() {
	// Create a graph
	node1 := &Node{Val: 1}
	node2 := &Node{Val: 2}
	node3 := &Node{Val: 3}
	node4 := &Node{Val: 4}

	node1.Neighbors = []*Node{node2, node4}
	node2.Neighbors = []*Node{node1, node3}
	node3.Neighbors = []*Node{node2, node4}
	node4.Neighbors = []*Node{node1, node3}

	// Clone the graph
	clone := cloneGraph(node1)

	// Print the cloned graph
	fmt.Println(clone)
}

```
In this code, we define a Node struct to represent a node in the graph. Each node has a Val field and a list of Neighbors which are pointers to other nodes.

The cloneGraph function takes a node as input and returns a cloned copy of the graph. It uses a depth-first search (DFS) approach to traverse the original graph and create a clone of each node and its neighbors. It maintains a visited map to keep track of the cloned nodes to avoid duplicate cloning.

The dfs function is a helper function that performs the actual DFS traversal. It clones the current node, adds it to the visited map, and recursively clones its neighbors by calling dfs on each neighbor. The cloned neighbors are appended to the Neighbors list of the cloned node.

In the main function, we create a sample graph with four nodes and their connections. We then call cloneGraph to clone the graph and store the cloned copy in the clone variable. Finally, we print the cloned graph to verify the cloning process.

Please note that this code assumes a connected undirected graph. If your graph contains directed edges or disconnected components, additional modifications may be required.

#### Pacific Atlantic Water Flow

```
package main

import "fmt"

func pacificAtlantic(matrix [][]int) [][]int {
	if len(matrix) == 0 {
		return nil
	}

	m, n := len(matrix), len(matrix[0])
	pacific := make([][]bool, m)
	atlantic := make([][]bool, m)
	for i := 0; i < m; i++ {
		pacific[i] = make([]bool, n)
		atlantic[i] = make([]bool, n)
	}

	for i := 0; i < m; i++ {
		dfs(matrix, pacific, i, 0, -1)
		dfs(matrix, atlantic, i, n-1, -1)
	}

	for j := 0; j < n; j++ {
		dfs(matrix, pacific, 0, j, -1)
		dfs(matrix, atlantic, m-1, j, -1)
	}

	result := make([][]int, 0)
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if pacific[i][j] && atlantic[i][j] {
				result = append(result, []int{i, j})
			}
		}
	}

	return result
}

func dfs(matrix [][]int, ocean [][]bool, row, col, prevHeight int) {
	m, n := len(matrix), len(matrix[0])
	if row < 0 || col < 0 || row >= m || col >= n || ocean[row][col] || matrix[row][col] < prevHeight {
		return
	}

	ocean[row][col] = true
	directions := [][]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}
	for _, dir := range directions {
		dfs(matrix, ocean, row+dir[0], col+dir[1], matrix[row][col])
	}
}

func main() {
	// Example usage
	matrix := [][]int{
		{1, 2, 2, 3, 5},
		{3, 2, 3, 4, 4},
		{2, 4, 5, 3, 1},
		{6, 7, 1, 4, 5},
		{5, 1, 1, 2, 4},
	}

	result := pacificAtlantic(matrix)
	fmt.Println(result)
}

```
In this code, the pacificAtlantic function takes a 2D matrix representing the heights of land and water levels and returns a list of coordinates (row, column) where the water can flow to both the Pacific and Atlantic oceans.

We create two boolean matrices pacific and atlantic of the same size as the input matrix to track the reachability of water from the Pacific and Atlantic oceans respectively. We initialize them as false.

We then perform depth-first search (DFS) from each border cell (Pacific and Atlantic) and mark the cells that can be reached by water. We use the dfs helper function to perform the DFS traversal. It checks if the current cell is valid, not already visited, and has a higher or equal height compared to the previous cell. If so, it marks the cell as visited and recursively calls dfs on its neighboring cells.

After performing the DFS on both oceans, we iterate through the matrix and add the cells that can be reached by both oceans to the result list.

In the main function, we create an example matrix and call the pacificAtlantic function to get the result. Finally, we print the result to verify the solution.

Please note that the code assumes the matrix is non-empty and rectangular. Additional checks and modifications may be required to handle different edge cases.

#### Course Schedule

```
package main

import "fmt"

func canFinish(numCourses int, prerequisites [][]int) bool {
	adjacencyList := make([][]int, numCourses)
	inDegree := make([]int, numCourses)

	// Build adjacency list and calculate in-degrees
	for _, prerequisite := range prerequisites {
		course, prerequisiteCourse := prerequisite[0], prerequisite[1]
		adjacencyList[prerequisiteCourse] = append(adjacencyList[prerequisiteCourse], course)
		inDegree[course]++
	}

	queue := make([]int, 0)

	// Add courses with no prerequisites to the queue
	for course, degree := range inDegree {
		if degree == 0 {
			queue = append(queue, course)
		}
	}

	count := 0 // Count of completed courses

	for len(queue) > 0 {
		currentCourse := queue[0]
		queue = queue[1:] // Dequeue

		count++ // Increment the count of completed courses

		// Iterate through the courses dependent on the current course
		for _, dependentCourse := range adjacencyList[currentCourse] {
			inDegree[dependentCourse]-- // Decrement the in-degree of dependent course
			if inDegree[dependentCourse] == 0 {
				queue = append(queue, dependentCourse) // Enqueue the course with no remaining prerequisites
			}
		}
	}

	return count == numCourses
}

func main() {
	// Example usage
	numCourses := 4
	prerequisites := [][]int{{1, 0}, {2, 0}, {3, 1}, {3, 2}}

	canFinish := canFinish(numCourses, prerequisites)
	fmt.Println(canFinish)
}

```

In this code, the canFinish function takes the number of courses numCourses and a list of prerequisites prerequisites as input. It returns a boolean value indicating whether it is possible to finish all the courses.

We first create an adjacency list to represent the dependencies between courses. Each course is represented as a node, and the prerequisite courses are stored as neighbors in the adjacency list. We also maintain an array inDegree to keep track of the in-degrees of each course.

We then iterate through the prerequisites to build the adjacency list and calculate the in-degrees of each course.

We use a queue to perform a topological sorting of the courses. We enqueue the courses with no prerequisites (in-degree = 0) into the queue initially. We dequeue a course, increment the count of completed courses, and decrement the in-degree of its dependent courses. If a dependent course has no remaining prerequisites (in-degree = 0), we enqueue it.

Finally, we check if the count of completed courses is equal to the total number of courses. If they are equal, it means all courses can be finished, and we return true. Otherwise, there are some courses that cannot be completed due to cyclic dependencies, and we return false.

In the main function, we create an example scenario with 4 courses and their prerequisites. We call the canFinish function and print the result.

Please note that the code assumes valid input, and additional checks may be required for edge cases and error handling.


#### Number of Connected Components In An Undirected Graph

```
package main

import "fmt"

type Graph struct {
	vertices   int
	adjacency  [][]int
	visited    []bool
}

func NewGraph(vertices int) *Graph {
	adjacency := make([][]int, vertices)
	visited := make([]bool, vertices)
	return &Graph{
		vertices:  vertices,
		adjacency: adjacency,
		visited:   visited,
	}
}

func (g *Graph) AddEdge(u, v int) {
	g.adjacency[u] = append(g.adjacency[u], v)
	g.adjacency[v] = append(g.adjacency[v], u)
}

func (g *Graph) DFS(v int) {
	g.visited[v] = true

	// Visit all adjacent vertices
	for _, neighbor := range g.adjacency[v] {
		if !g.visited[neighbor] {
			g.DFS(neighbor)
		}
	}
}

func (g *Graph) CountConnectedComponents() int {
	count := 0

	// Perform DFS on each unvisited vertex
	for v := 0; v < g.vertices; v++ {
		if !g.visited[v] {
			g.DFS(v)
			count++
		}
	}

	return count
}

func main() {
	// Example usage
	vertices := 6
	edges := [][]int{{0, 1}, {1, 2}, {3, 4}, {4, 5}}

	graph := NewGraph(vertices)

	// Add edges to the graph
	for _, edge := range edges {
		u, v := edge[0], edge[1]
		graph.AddEdge(u, v)
	}

	// Count the number of connected components
	count := graph.CountConnectedComponents()
	fmt.Println(count)
}

```

In this code, we define a Graph struct that represents an undirected graph. It has fields for the number of vertices, the adjacency list, and a boolean array to keep track of visited vertices.

The NewGraph function creates a new instance of the Graph with the specified number of vertices. The AddEdge method adds an undirected edge between two vertices.

The DFS method performs depth-first search starting from a given vertex v. It marks the vertex as visited and recursively visits all its adjacent vertices.

The CountConnectedComponents method counts the number of connected components in the graph. It performs DFS on each unvisited vertex and increments the count whenever a new connected component is encountered.

In the main function, we create an example scenario with 6 vertices and their corresponding edges. We create a new Graph, add the edges, and then call the CountConnectedComponents method to get the count of connected components in the graph.

Please note that the code assumes valid input and does not handle error cases or edge conditions. Additional checks and error handling may be required based on specific requirements.



#### Graph Valid Tree

```
package main

type Graph struct {
	vertices  int
	adjacency [][]int
	visited   []bool
}

func NewGraph(vertices int) *Graph {
	adjacency := make([][]int, vertices)
	visited := make([]bool, vertices)
	return &Graph{
		vertices:  vertices,
		adjacency: adjacency,
		visited:   visited,
	}
}

func (g *Graph) AddEdge(u, v int) {
	g.adjacency[u] = append(g.adjacency[u], v)
	g.adjacency[v] = append(g.adjacency[v], u)
}

func (g *Graph) DFS(v, parent int) bool {
	g.visited[v] = true

	// Visit all adjacent vertices
	for _, neighbor := range g.adjacency[v] {
		if !g.visited[neighbor] {
			if g.DFS(neighbor, v) == false {
				return false
			}
		} else if neighbor != parent {
			return false
		}
	}

	return true
}

func (g *Graph) IsValidTree() bool {
	// Check if the graph is connected
	if g.DFS(0, -1) == false {
		return false
	}

	// Check if there are no cycles
	for v := 0; v < g.vertices; v++ {
		if !g.visited[v] {
			return false
		}
	}

	return true
}

func main() {
	// Example usage
	vertices := 5
	edges := [][]int{{0, 1}, {0, 2}, {0, 3}, {1, 4}}

	graph := NewGraph(vertices)

	// Add edges to the graph
	for _, edge := range edges {
		u, v := edge[0], edge[1]
		graph.AddEdge(u, v)
	}

	// Check if the graph is a valid tree
	validTree := graph.IsValidTree()
	fmt.Println(validTree)
}

```

In this code, we define a Graph struct that represents an undirected graph. It has fields for the number of vertices, the adjacency list, and a boolean array to keep track of visited vertices.

The NewGraph function creates a new instance of the Graph with the specified number of vertices. The AddEdge method adds an undirected edge between two vertices.

The DFS method performs depth-first search starting from a given vertex v. It marks the vertex as visited and recursively visits all its adjacent vertices. During the traversal, it checks if there are any cycles or if there is an edge to a previously visited vertex other than its parent.

The IsValidTree method checks if the graph is a valid tree by performing a DFS starting from vertex 0 (assuming it is the root). It checks if the graph is connected and there are no cycles. Finally, it checks if all vertices have been visited.

In the main function, we create an example scenario with 5 vertices and their corresponding edges. We create a new Graph, add the edges, and then call the IsValidTree method to check if the graph is a valid tree.

Please note that the code assumes valid input and does not handle error cases or edge conditions. Additional checks and error handling may be required based on specific requirements.



