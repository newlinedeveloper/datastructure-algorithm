
### Greedy programming

#### Maximum Subarray

```
package main

import "fmt"

func maxSubArray(nums []int) int {
	if len(nums) == 0 {
		return 0
	}

	maxSum := nums[0]
	currentSum := nums[0]

	for i := 1; i < len(nums); i++ {
		currentSum = max(nums[i], currentSum+nums[i])
		maxSum = max(maxSum, currentSum)
	}

	return maxSum
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	nums := []int{-2, 1, -3, 4, -1, 2, 1, -5, 4}
	maxSum := maxSubArray(nums)
	fmt.Printf("Maximum Subarray Sum: %d\n", maxSum)
}


```

In this code, the maxSubArray function takes an integer slice nums as input and returns the maximum sum of a contiguous subarray within the given slice.

We initialize two variables maxSum and currentSum to the first element of the slice nums. We iterate through the remaining elements of nums starting from the second element.

For each element, we compare the sum of the current element and the previous subarray sum currentSum+nums[i] with the current element nums[i] itself. We update the currentSum to be the maximum of these two values.

We also update the maxSum to be the maximum of the previous maxSum and the updated currentSum. This way, we keep track of the maximum sum encountered so far.

Finally, we return the maxSum as the maximum subarray sum.

In the main function, we create an example integer slice nums and call the maxSubArray function with this slice. We then print the maximum subarray sum.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.


#### Jump Game

```
package main

import "fmt"

func canJump(nums []int) bool {
	lastPos := len(nums) - 1
	for i := len(nums) - 1; i >= 0; i-- {
		if i+nums[i] >= lastPos {
			lastPos = i
		}
	}
	return lastPos == 0
}

func main() {
	nums := []int{2, 3, 1, 1, 4}
	result := canJump(nums)
	fmt.Printf("Can jump to the last index: %v\n", result)
}


```
In this code, the canJump function takes an integer slice nums as input and returns a boolean value indicating whether it's possible to jump to the last index of the array.

We initialize the lastPos variable to the last index of the array. We iterate through the array backwards, starting from the second-to-last element.

For each element, we check if we can reach or surpass the lastPos from the current position. If so, we update the lastPos to the current index, indicating that we can jump to this position.

By traversing the array in reverse, we iteratively update the lastPos to the leftmost index from which we can reach the last index. If the lastPos eventually becomes 0, it means we can jump to the last index from the first position.

Finally, we return lastPos == 0 to indicate whether it's possible to jump to the last index.

In the main function, we create an example integer slice nums and call the canJump function with this slice. We then print whether it's possible to jump to the last index.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.


