### Backtracking

#### Combination Sum

```
package main

import "fmt"

func combinationSum(candidates []int, target int) [][]int {
	result := [][]int{}
	backtrack(&result, []int{}, candidates, target, 0)
	return result
}

func backtrack(result *[][]int, current []int, candidates []int, target int, start int) {
	if target < 0 {
		return
	}
	if target == 0 {
		// Add the current combination to the result
		comb := make([]int, len(current))
		copy(comb, current)
		*result = append(*result, comb)
		return
	}
	for i := start; i < len(candidates); i++ {
		// Include the current candidate in the combination
		current = append(current, candidates[i])
		// Continue exploring with the updated combination and reduced target
		backtrack(result, current, candidates, target-candidates[i], i)
		// Remove the last candidate to backtrack
		current = current[:len(current)-1]
	}
}

func main() {
	candidates := []int{2, 3, 6, 7}
	target := 7

	result := combinationSum(candidates, target)
	fmt.Println(result)
}

```
In this code, the combinationSum function takes a slice of candidates and a target integer as input and returns a slice of slices representing all combinations of candidates that sum up to the target.

The backtrack function is a helper function that performs the backtracking algorithm. It takes the current combination, candidates, target, and the start index as parameters. It explores all possible combinations by trying each candidate and recursively exploring the remaining candidates and updated target. If the target becomes 0, it means we have found a valid combination, so we add it to the result. If the target becomes negative, we backtrack and try a different candidate.

In the main function, we define a list of candidates and a target, and then call the combinationSum function to solve the problem. The resulting combinations are printed to the console.



#### Word Search

```
package main

import "fmt"

func exist(board [][]byte, word string) bool {
	if len(board) == 0 || len(board[0]) == 0 {
		return false
	}

	m, n := len(board), len(board[0])
	visited := make([][]bool, m)
	for i := range visited {
		visited[i] = make([]bool, n)
	}

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if backtrack(board, visited, i, j, word, 0) {
				return true
			}
		}
	}

	return false
}

func backtrack(board [][]byte, visited [][]bool, row, col int, word string, index int) bool {
	if index == len(word) {
		return true
	}

	m, n := len(board), len(board[0])

	if row < 0 || row >= m || col < 0 || col >= n || visited[row][col] || board[row][col] != word[index] {
		return false
	}

	visited[row][col] = true
	if backtrack(board, visited, row+1, col, word, index+1) ||
		backtrack(board, visited, row-1, col, word, index+1) ||
		backtrack(board, visited, row, col+1, word, index+1) ||
		backtrack(board, visited, row, col-1, word, index+1) {
		return true
	}

	visited[row][col] = false
	return false
}

func main() {
	board := [][]byte{
		{'A', 'B', 'C', 'E'},
		{'S', 'F', 'C', 'S'},
		{'A', 'D', 'E', 'E'},
	}
	word := "ABCCED"

	result := exist(board, word)
	fmt.Println(result)
}


```
In this code, the exist function takes a 2D board and a target word as input and returns a boolean value indicating whether the word exists in the board or not.

The backtrack function is a helper function that performs the backtracking algorithm. It takes the board, visited matrix, current row and column indices, the target word, and the current index of the word character as parameters. It checks if the current cell matches the current character of the word, and if so, marks the cell as visited. It then recursively explores the neighboring cells in a depth-first manner to find the next character of the word. If a valid path is found, it returns true. Otherwise, it backtracks by unmarking the current cell and returns false.

In the main function, we define a 2D board and a target word, and then call the exist function to solve the problem. The result is printed to the console.



