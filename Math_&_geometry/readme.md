### Math & Geometry


#### Rotate Image

```
package main

import "fmt"

func rotate(matrix [][]int) {
	n := len(matrix)

	// Transpose the matrix
	for i := 0; i < n; i++ {
		for j := i; j < n; j++ {
			matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
		}
	}

	// Reverse each row
	for i := 0; i < n; i++ {
		for j := 0; j < n/2; j++ {
			matrix[i][j], matrix[i][n-1-j] = matrix[i][n-1-j], matrix[i][j]
		}
	}
}

func main() {
	matrix := [][]int{
		{1, 2, 3},
		{4, 5, 6},
		{7, 8, 9},
	}

	fmt.Println("Original Matrix:")
	printMatrix(matrix)

	rotate(matrix)

	fmt.Println("Rotated Matrix:")
	printMatrix(matrix)
}

func printMatrix(matrix [][]int) {
	for _, row := range matrix {
		for _, val := range row {
			fmt.Printf("%d ", val)
		}
		fmt.Println()
	}
}


```

In this code, the rotate function takes a square matrix as input and performs an in-place rotation of the matrix by 90 degrees clockwise.

The rotate function follows the following steps:

Get the size of the matrix n (assuming the matrix is a square matrix).
Transpose the matrix by swapping the elements across the diagonal. This step effectively reflects the matrix along its main diagonal.
Reverse each row of the transposed matrix. This step completes the rotation by flipping each row horizontally.
In the main function, we create an example matrix matrix and print the original matrix. Then, we call the rotate function with this matrix and print the rotated matrix.

The printMatrix function is a utility function used to print the matrix in a readable format.

Please note that the code assumes a square matrix and may require modifications to handle non-square matrices or handle edge cases based on specific requirements.

#### Spiral Matrix

```
package main

import "fmt"

func spiralOrder(matrix [][]int) []int {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return nil
	}

	m, n := len(matrix), len(matrix[0])
	result := make([]int, m*n)
	left, right, top, bottom := 0, n-1, 0, m-1
	idx := 0

	for left <= right && top <= bottom {
		// Traverse top row
		for i := left; i <= right; i++ {
			result[idx] = matrix[top][i]
			idx++
		}
		top++

		// Traverse right column
		for i := top; i <= bottom; i++ {
			result[idx] = matrix[i][right]
			idx++
		}
		right--

		// Traverse bottom row
		if top <= bottom {
			for i := right; i >= left; i-- {
				result[idx] = matrix[bottom][i]
				idx++
			}
			bottom--
		}

		// Traverse left column
		if left <= right {
			for i := bottom; i >= top; i-- {
				result[idx] = matrix[i][left]
				idx++
			}
			left++
		}
	}

	return result
}

func main() {
	matrix := [][]int{
		{1, 2, 3},
		{4, 5, 6},
		{7, 8, 9},
	}

	fmt.Println("Matrix:")
	printMatrix(matrix)

	result := spiralOrder(matrix)

	fmt.Println("Spiral Order:")
	fmt.Println(result)
}

func printMatrix(matrix [][]int) {
	for _, row := range matrix {
		for _, val := range row {
			fmt.Printf("%d ", val)
		}
		fmt.Println()
	}
}


```

In this code, the spiralOrder function takes a 2D matrix as input and returns the elements of the matrix in spiral order as a 1D array.

The spiralOrder function follows the following steps:

Initialize four variables to track the boundaries of the matrix: left, right, top, and bottom.
Iterate in a spiral order by traversing the top row, right column, bottom row, and left column of the matrix.
Append each element encountered during the traversal to the result array.
Update the boundary variables accordingly to narrow down the traversal area.
In the main function, we create an example matrix matrix and print the original matrix. Then, we call the spiralOrder function with this matrix and print the elements in spiral order.

The printMatrix function is a utility function used to print the matrix in a readable format.

Please note that the code assumes a non-empty matrix and may require modifications to handle edge cases or specific requirements.


#### Set Matrix Zeroes

```
package main

import "fmt"

func setZeroes(matrix [][]int) {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return
	}

	m, n := len(matrix), len(matrix[0])
	firstRowZero, firstColZero := false, false

	// Check if the first row has any zero
	for j := 0; j < n; j++ {
		if matrix[0][j] == 0 {
			firstRowZero = true
			break
		}
	}

	// Check if the first column has any zero
	for i := 0; i < m; i++ {
		if matrix[i][0] == 0 {
			firstColZero = true
			break
		}
	}

	// Use the first row and column to mark the zero positions
	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			if matrix[i][j] == 0 {
				matrix[i][0] = 0
				matrix[0][j] = 0
			}
		}
	}

	// Set the corresponding rows to zeros
	for i := 1; i < m; i++ {
		if matrix[i][0] == 0 {
			for j := 1; j < n; j++ {
				matrix[i][j] = 0
			}
		}
	}

	// Set the corresponding columns to zeros
	for j := 1; j < n; j++ {
		if matrix[0][j] == 0 {
			for i := 1; i < m; i++ {
				matrix[i][j] = 0
			}
		}
	}

	// Set the first row and column to zeros if necessary
	if firstRowZero {
		for j := 0; j < n; j++ {
			matrix[0][j] = 0
		}
	}

	if firstColZero {
		for i := 0; i < m; i++ {
			matrix[i][0] = 0
		}
	}
}

func main() {
	matrix := [][]int{
		{1, 1, 1},
		{1, 0, 1},
		{1, 1, 1},
	}

	fmt.Println("Original Matrix:")
	printMatrix(matrix)

	setZeroes(matrix)

	fmt.Println("Matrix after Setting Zeroes:")
	printMatrix(matrix)
}

func printMatrix(matrix [][]int) {
	for _, row := range matrix {
		for _, val := range row {
			fmt.Printf("%d ", val)
		}
		fmt.Println()
	}
}


```

In this code, the setZeroes function takes a 2D matrix as input and modifies the matrix in-place by setting the entire row and column to zero if any element in the corresponding row or column is zero.

The setZeroes function follows the following steps:

Check if the first row has any zero and store the result in the firstRowZero variable.
Check if the first column has any zero and store the result in the firstColZero variable.
Use the first row and column to mark the zero positions by setting the corresponding elements to zero.
Traverse the matrix starting from the second row and second column. If an element is zero, set the corresponding first row element and first column element to zero.
Traverse the matrix again, starting




