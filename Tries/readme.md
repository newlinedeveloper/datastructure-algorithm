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


