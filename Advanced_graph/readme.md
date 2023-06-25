### Advanced Graph

#### Alien Dictionary

```
package main

import (
	"fmt"
	"strings"
)

type Graph struct {
	vertices  map[byte]bool
	adjacency map[byte][]byte
	visited   map[byte]bool
	result    []byte
}

func NewGraph() *Graph {
	vertices := make(map[byte]bool)
	adjacency := make(map[byte][]byte)
	visited := make(map[byte]bool)
	return &Graph{
		vertices:  vertices,
		adjacency: adjacency,
		visited:   visited,
		result:    make([]byte, 0),
	}
}

func (g *Graph) AddVertex(v byte) {
	g.vertices[v] = true
}

func (g *Graph) AddEdge(u, v byte) {
	g.adjacency[u] = append(g.adjacency[u], v)
}

func (g *Graph) TopologicalSort(v byte) {
	g.visited[v] = true

	for _, neighbor := range g.adjacency[v] {
		if !g.visited[neighbor] {
			g.TopologicalSort(neighbor)
		}
	}

	g.result = append(g.result, v)
}

func AlienDictionary(words []string) string {
	graph := NewGraph()

	// Add all unique characters as vertices
	for _, word := range words {
		for _, char := range word {
			graph.AddVertex(byte(char))
		}
	}

	// Build the adjacency list based on the given words
	for i := 0; i < len(words)-1; i++ {
		word1 := words[i]
		word2 := words[i+1]
		minLen := min(len(word1), len(word2))

		for j := 0; j < minLen; j++ {
			char1 := word1[j]
			char2 := word2[j]

			if char1 != char2 {
				graph.AddEdge(byte(char1), byte(char2))
				break
			}
		}
	}

	// Perform topological sorting
	for v := range graph.vertices {
		if !graph.visited[v] {
			graph.TopologicalSort(v)
		}
	}

	// Reverse the result to get the correct order
	reverseString(graph.result)

	return string(graph.result)
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func reverseString(s []byte) {
	i := 0
	j := len(s) - 1

	for i < j {
		s[i], s[j] = s[j], s[i]
		i++
		j--
	}
}

func main() {
	words := []string{"wrt", "wrf", "er", "ett", "rftt"}

	order := AlienDictionary(words)
	fmt.Println(order) // Output: "wertf"
}


```

In this code, we define a Graph struct that represents a directed graph. It has fields for the set of vertices, the adjacency list, the visited nodes, and the result list.

The NewGraph function creates a new instance of the Graph with the empty sets and lists.

The AddVertex method adds a new vertex to the graph by adding it to the set of vertices.

The AddEdge method adds a directed edge from vertex u to vertex v by appending v to the adjacency list of u.

The TopologicalSort method performs a recursive depth-first search to perform the topological sort. It marks the current vertex as visited, recursively visits all unvisited neighbors, and appends the current vertex to the result list.

The AlienDictionary function takes a slice of words and builds the graph based on the order of characters in the words. It adds all unique characters as vertices and adds edges between adjacent characters in different words. Then it performs the topological sort to obtain the correct order of characters.

In the main function, we create an example scenario with a slice of words. We call the AlienDictionary function with the words and print the resulting order.
