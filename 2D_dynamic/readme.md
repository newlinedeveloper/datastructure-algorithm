
### 2D Dynamic programming 


#### Unique Paths

```
package main

import "fmt"

func uniquePaths(m int, n int) int {
	dp := make([][]int, m)

	for i := 0; i < m; i++ {
		dp[i] = make([]int, n)
	}

	// Initialize the boundary values
	for i := 0; i < m; i++ {
		dp[i][0] = 1
	}

	for j := 0; j < n; j++ {
		dp[0][j] = 1
	}

	// Calculate the number of unique paths for each cell
	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			dp[i][j] = dp[i-1][j] + dp[i][j-1]
		}
	}

	return dp[m-1][n-1]
}

func main() {
	m := 3
	n := 7
	paths := uniquePaths(m, n)
	fmt.Printf("Number of Unique Paths for a %dx%d Grid: %d\n", m, n, paths)
}


```

In this code, the uniquePaths function takes the dimensions m and n of a grid as input and returns the number of unique paths from the top-left corner to the bottom-right corner of the grid.

We create a 2D dynamic programming array dp with dimensions m rows and n columns. Each cell dp[i][j] represents the number of unique paths to reach the cell at position (i, j).

We initialize the boundary values, where dp[i][0] = 1 for all rows and dp[0][j] = 1 for all columns, as there is only one way to reach the cells in the first row and first column.

Then, we iterate through the remaining cells in the grid (starting from (1, 1)) and calculate the number of unique paths for each cell by summing the number of paths from the cell above and the cell to the left.

Finally, we return the value in the bottom-right corner of the grid, which represents the total number of unique paths.

In the main function, we create an example grid with dimensions m = 3 and n = 7. We call the uniquePaths function with these dimensions and print the number of unique paths.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.


#### Longest Common Subsequence

```
package main

import (
	"fmt"
)

func longestCommonSubsequence(text1 string, text2 string) int {
	m, n := len(text1), len(text2)

	// Create a 2D dynamic programming table
	dp := make([][]int, m+1)
	for i := range dp {
		dp[i] = make([]int, n+1)
	}

	// Calculate the lengths of the longest common subsequences
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if text1[i-1] == text2[j-1] {
				dp[i][j] = dp[i-1][j-1] + 1
			} else {
				dp[i][j] = max(dp[i-1][j], dp[i][j-1])
			}
		}
	}

	return dp[m][n]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	text1 := "abcde"
	text2 := "ace"

	lcs := longestCommonSubsequence(text1, text2)
	fmt.Printf("Length of Longest Common Subsequence: %d\n", lcs)
}


```
In this code, the longestCommonSubsequence function takes two strings text1 and text2 as input and returns the length of the longest common subsequence between the two strings.

We create a 2D dynamic programming table dp of size (m+1) x (n+1), where m and n are the lengths of text1 and text2, respectively.

We iterate through the characters of text1 and text2, and for each pair of characters, we check if they are equal. If they are equal, it means we have found a new character that contributes to the common subsequence. In that case, we increment the length of the subsequence by 1.

If the characters are not equal, we take the maximum of the lengths of the subsequences obtained by excluding each of the characters and store it in the dynamic programming table.

Finally, we return the value in the bottom-right corner of the table, which represents the length of the longest common subsequence.

In the main function, we create two example strings text1 and text2. We call the longestCommonSubsequence function with these strings and print the length of the longest common subsequence.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.
