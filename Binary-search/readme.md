### Binary Search


#### Problem 1 - Easy : Binary Search

```
func binarySearch(nums []int, target int) int {
    left, right := 0, len(nums)-1

    for left <= right {
        mid := left + (right-left)/2

        if nums[mid] == target {
            return mid
        } else if nums[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }

    return -1
}

```
Here, the binarySearch function takes a sorted slice of integers nums and a target integer target, and returns the index of the target integer in the slice if it exists, or -1 otherwise.

The function initializes two variables left and right to the indices of the first and last elements in the slice, respectively. It then enters a loop that continues while left is less than or equal to right. On each iteration of the loop, the function calculates the index of the middle element of the slice using the formula (left + right) / 2.

If the middle element is equal to the target integer, the function returns the index of the middle element. If the middle element is less than the target integer, the function updates left to mid + 1 to search the right half of the slice. If the middle element is greater than the target integer, the function updates right to mid - 1 to search the left half of the slice.

If the target integer is not found in the slice, the function returns -1.

This algorithm has a time complexity of O(log n), where n is the length of the input slice. The space complexity is O(1), since the function only uses a constant amount of extra space.


Here's an example of how to use the binarySearch function:

```
nums := []int{1, 3, 5, 7, 9}
target := 5
index := binarySearch(nums, target)
fmt.Println(index) // Expected output: 2


```
In this example, the nums slice is sorted and contains integers from 1 to 9. The target variable is set to 5, which is an element in the slice. The expected output is 2, which is the index of the target integer in the nums slice.

#### Problem 2 - Medium : Search a 2D Matrix

```
func searchMatrix(matrix [][]int, target int) bool {
    if len(matrix) == 0 {
        return false
    }
    
    m, n := len(matrix), len(matrix[0])
    left, right := 0, m*n-1
    
    for left <= right {
        mid := left + (right-left)/2
        row, col := mid/n, mid%n
        
        if matrix[row][col] == target {
            return true
        } else if matrix[row][col] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    
    return false
}


```

Here, the searchMatrix function takes a matrix matrix of integers and a target integer target, and returns a boolean value indicating whether the target integer is in the matrix.

The function first checks if the matrix is empty, and returns false if it is. It then initializes two variables m and n to the number of rows and columns in the matrix, respectively, and initializes two variables left and right to the indices of the first and last elements in the matrix, respectively, treating the matrix as a one-dimensional array.

The function then enters a loop that continues while left is less than or equal to right. On each iteration of the loop, the function calculates the index of the middle element of the matrix using the formula (left + right) / 2, and computes the corresponding row and column indices using integer division and modulus operations.

If the middle element is equal to the target integer, the function returns true. If the middle element is less than the target integer, the function updates left to mid + 1 to search the right half of the matrix. If the middle element is greater than the target integer, the function updates right to mid - 1 to search the left half of the matrix.

If the target integer is not found in the matrix, the function returns false.

This algorithm has a time complexity of O(log (mn)), where m and n are the number of rows and columns in the matrix, respectively. The space complexity is O(1), since the function only uses a constant amount of extra space.

Here's an example of how to use the searchMatrix function:

```
matrix := [][]int{{1,3,5,7},{10,11,16,20},{23,30,34,50}}
target := 3
found := searchMatrix(matrix, target)
fmt.Println(found) // Expected output: true


```

In this example, the matrix variable is a 3x4 matrix of integers, and the target variable is set to 3, which is an element in the matrix. The expected output is true, indicating that the target integer was found in the matrix.


#### Problem 3 - Medium : Koko Eating Bananas

```
func minEatingSpeed(piles []int, h int) int {
    // Compute the maximum pile size
    maxPile := 0
    for _, p := range piles {
        if p > maxPile {
            maxPile = p
        }
    }
    
    // Perform binary search to find the minimum eating speed
    left, right := 1, maxPile
    for left <= right {
        mid := left + (right-left)/2
        
        // Check if the current eating speed is valid
        if canEatAll(piles, h, mid) {
            // If it is, we can try a lower eating speed
            right = mid - 1
        } else {
            // If it isn't, we need to try a higher eating speed
            left = mid + 1
        }
    }
    
    return left
}

// Helper function to check if a given eating speed is valid
func canEatAll(piles []int, h int, k int) bool {
    time := 0
    for _, p := range piles {
        time += (p + k - 1) / k // Round up
        if time > h {
            return false
        }
    }
    return true
}


```

Here, the minEatingSpeed function takes a slice of integers piles representing the sizes of banana piles and an integer h representing the maximum number of hours Koko can spend eating bananas. The function returns an integer value representing the minimum eating speed that Koko can use to eat all the bananas within h hours.

The function first computes the maximum pile size by iterating over all the piles. It then initializes two variables left and right to 1 and maxPile, respectively, representing the range of possible eating speeds. We perform binary search on this range to find the minimum eating speed that Koko can use to eat all the bananas within h hours.

On each iteration of the binary search loop, we calculate the middle eating speed using the formula mid := left + (right-left)/2. We then check if the current eating speed mid is valid by calling the helper function canEatAll with the piles, h, and mid arguments. If the function returns true, we know that Koko can eat all the bananas within h hours with an eating speed of mid, so we update right = mid - 1 to try a lower eating speed. If the function returns false, we know that Koko cannot eat all the bananas within h hours with an eating speed of mid, so we update left = mid + 1 to try a higher eating speed.

The helper function canEatAll takes the same piles, h, and k arguments as minEatingSpeed. It computes the total time required for Koko to eat all the bananas at the given eating speed k, and checks if this time is less than or equal to h. To compute the total time, we iterate over all the piles and add the time required to eat each pile using the formula (p + k - 1) / k, which rounds up the division to the nearest integer.

This algorithm has a time complexity of O(n log m), where n is the number of banana piles and m is the maximum pile size. The space complexity is O(1), since the function only uses a constant amount of extra space.

To test the minEatingSpeed function, you can call it with some sample inputs and check if it returns the expected outputs. Here are some examples:

```
func main() {
    piles1 := []int{3, 6, 7, 11}
    h1 := 8
    fmt.Println(minEatingSpeed(piles1, h1)) // Expected output: 4
    
    piles2 := []int{30, 11, 23, 4, 20}
    h2 := 5
    fmt.Println(minEatingSpeed(piles2, h2)) // Expected output: 30
    
    piles3 := []int{30, 11, 23, 4, 20}
    h3 := 6
    fmt.Println(minEatingSpeed(piles3, h3)) // Expected output: 23
}


```

In the first example, Koko can eat all the bananas within 8 hours using an eating speed of 4, since she can eat the first pile in 1 hour, the second pile in 2 hours, the third pile in 2 hours, and the fourth pile in 3 hours.

In the second example, Koko can eat all the bananas within 5 hours using an eating speed of 30, since she can eat any of the piles in less than 5 hours.

In the third example, Koko cannot eat all the bananas within 6 hours using an eating speed of 23, since she would need 1 hour to eat the first pile, 1 hour to eat the second pile, and 2 hours to eat the third pile, which would exceed the available time.

You can also try other test cases with different inputs to make sure the function works correctly.






#### Problem 4 - Medium : Find Minimum In Rotated Sorted Array


#### Problem 5 - Medium : Search In Rotated Sorted Array


#### Problem 6 - Medium : Time Based Key Value Store


#### Problem 7 - Hard : Median of Two Sorted Arrays
