### Tries Datastructure

#### Implement Trie Prefix Tree

```
package main

import "fmt"

type TrieNode struct {
	children map[rune]*TrieNode
	isEnd    bool
}

type Trie struct {
	root *TrieNode
}

func NewTrie() *Trie {
	return &Trie{
		root: &TrieNode{
			children: make(map[rune]*TrieNode),
			isEnd:    false,
		},
	}
}

func (t *Trie) Insert(word string) {
	node := t.root
	for _, char := range word {
		if _, ok := node.children[char]; !ok {
			node.children[char] = &TrieNode{
				children: make(map[rune]*TrieNode),
				isEnd:    false,
			}
		}
		node = node.children[char]
	}
	node.isEnd = true
}

func (t *Trie) Search(word string) bool {
	node := t.root
	for _, char := range word {
		if _, ok := node.children[char]; !ok {
			return false
		}
		node = node.children[char]
	}
	return node.isEnd
}

func (t *Trie) StartsWith(prefix string) bool {
	node := t.root
	for _, char := range prefix {
		if _, ok := node.children[char]; !ok {
			return false
		}
		node = node.children[char]
	}
	return true
}

func main() {
	trie := NewTrie()

	trie.Insert("apple")
	trie.Insert("app")
	trie.Insert("banana")

	fmt.Println(trie.Search("apple"))       // Output: true
	fmt.Println(trie.Search("app"))         // Output: true
	fmt.Println(trie.Search("banana"))      // Output: true
	fmt.Println(trie.Search("orange"))      // Output: false
	fmt.Println(trie.StartsWith("app"))     // Output: true
	fmt.Println(trie.StartsWith("orange"))  // Output: false
}

```
In this code, we define two structs: TrieNode represents a node in the Trie, and Trie represents the Trie itself. The TrieNode struct has a children map that maps characters to child nodes, and an isEnd boolean flag to mark the end of a word.

The Trie struct has a root pointer to the root node of the Trie.

The NewTrie function creates a new Trie with an initialized root node.

The Insert method inserts a word into the Trie by traversing each character of the word and creating new nodes if necessary.

The Search method searches for a word in the Trie by traversing each character of the word and checking if the corresponding child node exists.

The StartsWith method checks if any word in the Trie starts with the given prefix by traversing each character of the prefix and checking if the corresponding child node exists.

In the main function, we create a new Trie, insert some words ("apple", "app", "banana"), and perform various search operations to demonstrate the functionality of the Trie.


#### Design Add And Search Words Data Structure
```
package main

import "fmt"

type TrieNode struct {
	children map[rune]*TrieNode
	isEnd    bool
}

type WordDictionary struct {
	root *TrieNode
}

func Constructor() WordDictionary {
	return WordDictionary{
		root: &TrieNode{
			children: make(map[rune]*TrieNode),
			isEnd:    false,
		},
	}
}

func (this *WordDictionary) AddWord(word string) {
	node := this.root
	for _, char := range word {
		if _, ok := node.children[char]; !ok {
			node.children[char] = &TrieNode{
				children: make(map[rune]*TrieNode),
				isEnd:    false,
			}
		}
		node = node.children[char]
	}
	node.isEnd = true
}

func (this *WordDictionary) Search(word string) bool {
	return this.searchWord(this.root, word)
}

func (this *WordDictionary) searchWord(node *TrieNode, word string) bool {
	for i, char := range word {
		if char != '.' {
			if _, ok := node.children[char]; !ok {
				return false
			}
			node = node.children[char]
		} else {
			for _, child := range node.children {
				if this.searchWord(child, word[i+1:]) {
					return true
				}
			}
			return false
		}
	}
	return node.isEnd
}

func main() {
	wd := Constructor()

	wd.AddWord("bad")
	wd.AddWord("dad")
	wd.AddWord("mad")

	fmt.Println(wd.Search("pad"))   // Output: false
	fmt.Println(wd.Search("bad"))   // Output: true
	fmt.Println(wd.Search(".ad"))   // Output: true
	fmt.Println(wd.Search("b.."))   // Output: true
}

```

In this code, we define two structs: TrieNode represents a node in the Trie, and WordDictionary represents the data structure for adding and searching words.

The TrieNode struct has a children map that maps characters to child nodes, and an isEnd boolean flag to mark the end of a word.

The WordDictionary struct has a root pointer to the root node of the Trie.

The Constructor function creates a new instance of the WordDictionary with an initialized root node.

The AddWord method adds a word to the Trie by traversing each character of the word and creating new nodes if necessary.

The Search method searches for a word in the Trie by recursively traversing each character of the word. If a character is a dot ('.'), it searches all child nodes recursively. If a character is not a dot, it checks if the corresponding child node exists.

In the main function, we create a new WordDictionary, add some words ("bad", "dad", "mad"), and perform various search operations to demonstrate the functionality of the data structure.

### word search 2

**Problem Description:**
Given a 2D board of characters and a list of words, find all the words on the board.

**Solution Approach:**
1. The most common and efficient approach to solving this problem is to use the Trie data structure.
2. Build a Trie from the list of words you're given.
3. Traverse the board and, for each cell, see if you can find a word in the Trie.
4. To do this, you'll use Depth-First Search (DFS) to explore neighboring cells on the board.
5. Keep track of the current word you're forming while traversing the board.
6. If you find a word in the Trie, add it to your result list.

**Step-by-Step Implementation:**
1. Build a Trie from the list of words:
   - Start with an empty Trie.
   - Insert each word from the list into the Trie.

2. Traverse the board:
   - Iterate through each cell on the board.
   - For each cell, start a DFS traversal.
   - Keep track of the current word formed while traversing.
   - For each neighbor cell, check if the combination of letters forms a prefix in the Trie.
   - If it does, continue the DFS traversal.

3. Implement DFS:
   - Recursively explore all neighboring cells (up, down, left, and right).
   - Check if the current word formed so far exists in the Trie:
     - If it does, add it to your result list.
   - Mark the cell as visited to avoid using it again in the same word.
   - Backtrack by unmarking the cell.

4. After traversing the entire board, you will have collected all the words found in the Trie.

5. Return the list of found words.


```
package main

type TrieNode struct {
	children map[rune]*TrieNode
	isEnd    bool
}

func findWords(board [][]byte, words []string) []string {
	var result []string

	// Build the Trie from the list of words
	root := &TrieNode{children: make(map[rune]*TrieNode)}
	for _, word := range words {
		node := root
		for _, char := range word {
			if _, exists := node.children[char]; !exists {
				node.children[char] = &TrieNode{children: make(map[rune]*TrieNode)}
			}
			node = node.children[char]
		}
		node.isEnd = true
	}

	// Perform DFS on the board
	var dfs func(node *TrieNode, i, j int, path string)
	dfs = func(node *TrieNode, i, j int, path string) {
		char := board[i][j]
		currNode, exists := node.children[rune(char)]
		if !exists {
			return
		}

		path += string(char)
		if currNode.isEnd {
			result = append(result, path)
			currNode.isEnd = false
		}

		originalChar, board[i][j] = board[i][j], '#'
		dirs := [][2]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}
		for _, dir := range dirs {
			x, y := i+dir[0], j+dir[1]
			if x >= 0 && x < len(board) && y >= 0 && y < len(board[0]) && board[x][y] != '#' {
				dfs(currNode, x, y, path)
			}
		}
		board[i][j] = originalChar
	}

	for i := range board {
		for j := range board[i] {
			dfs(root, i, j, "")
		}
	}

	return result
}

func main() {
	// Example usage
	board := [][]byte{
		{'o', 'a', 'a', 'n'},
		{'e', 't', 'a', 'e'},
		{'i', 'h', 'k', 'r'},
		{'i', 'f', 'l', 'v'},
	}
	words := []string{"oath", "pea", "eat", "rain"}
	result := findWords(board, words)
	fmt.Println(result) // Output: ["oath","eat"]
}

```




