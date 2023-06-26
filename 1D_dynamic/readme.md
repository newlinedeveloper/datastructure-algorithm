### 1D dynamic programming

#### Climbing Stairs

```
package main

import "fmt"

func climbStairs(n int) int {
	if n <= 2 {
		return n
	}

	dp := make([]int, n+1)
	dp[1] = 1
	dp[2] = 2

	for i := 3; i <= n; i++ {
		dp[i] = dp[i-1] + dp[i-2]
	}

	return dp[n]
}

func main() {
	n := 5
	ways := climbStairs(n)
	fmt.Println("Number of ways to climb", n, "stairs:", ways)
}


```

In this code, the climbStairs function takes an integer n representing the number of stairs to climb. It uses a 1D dynamic programming approach to calculate the number of distinct ways to climb the stairs.

We initialize an array dp of size n+1 to store the number of ways at each step. We set the base cases dp[1] = 1 and dp[2] = 2.

Then, we iterate from i = 3 to n and calculate the number of ways at each step by summing the number of ways to reach the previous two steps, dp[i] = dp[i-1] + dp[i-2].

Finally, we return the number of ways to climb n stairs, dp[n].

In the main function, we create an example scenario with n = 5 stairs. We call the climbStairs function with n and print the number of ways to climb the stairs.

Please note that the code assumes a valid positive integer input for n. Additional checks and error handling may be required based on specific requirements.



#### House Robber

```
package main

import "fmt"

func rob(nums []int) int {
	if len(nums) == 0 {
		return 0
	}

	if len(nums) == 1 {
		return nums[0]
	}

	dp := make([]int, len(nums))
	dp[0] = nums[0]
	dp[1] = max(nums[0], nums[1])

	for i := 2; i < len(nums); i++ {
		dp[i] = max(dp[i-1], dp[i-2]+nums[i])
	}

	return dp[len(nums)-1]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	nums := []int{2, 7, 9, 3, 1}
	maxAmount := rob(nums)
	fmt.Println("Maximum amount that can be robbed:", maxAmount)
}


```
In this code, the rob function takes a slice of integers nums, which represents the amount of money in each house. It uses a 1D dynamic programming approach to calculate the maximum amount of money that can be robbed.

We handle the base cases where if there are no houses (len(nums) == 0), the maximum amount is 0, and if there is only one house (len(nums) == 1), the maximum amount is the amount in that house.

We initialize an array dp of size len(nums) to store the maximum amount at each house. We set the initial values dp[0] = nums[0] and dp[1] = max(nums[0], nums[1]).

Then, we iterate from i = 2 to len(nums)-1 and calculate the maximum amount at each house by considering whether to rob the current house or the previous house. The maximum amount is given by dp[i] = max(dp[i-1], dp[i-2]+nums[i]).

Finally, we return the maximum amount of money that can be robbed, which is dp[len(nums)-1].

In the main function, we create an example scenario with nums = []int{2, 7, 9, 3, 1} representing the amount of money in each house. We call the rob function with nums and print the maximum amount that can be robbed.

Please note that the code assumes valid input and non-negative amounts in the houses. Additional checks and error handling may be required based on specific requirements



#### House Robber II

```
package main

import (
	"fmt"
)

func rob(nums []int) int {
	if len(nums) == 0 {
		return 0
	}

	if len(nums) == 1 {
		return nums[0]
	}

	// Case 1: Rob the first house and exclude the last house
	maxAmount1 := robHelper(nums[:len(nums)-1])

	// Case 2: Exclude the first house and rob the last house
	maxAmount2 := robHelper(nums[1:])

	return max(maxAmount1, maxAmount2)
}

func robHelper(nums []int) int {
	prevMax, currMax := 0, 0

	for _, num := range nums {
		temp := currMax
		currMax = max(prevMax+num, currMax)
		prevMax = temp
	}

	return currMax
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	nums := []int{2, 3, 2}
	maxAmount := rob(nums)
	fmt.Println("Maximum amount that can be robbed:", maxAmount)
}


```
In this code, the rob function takes a slice of integers nums, which represents the amount of money in each house arranged in a circular manner. It uses the 1D dynamic programming approach to calculate the maximum amount of money that can be robbed.

We handle the base cases where if there are no houses (len(nums) == 0), the maximum amount is 0, and if there is only one house (len(nums) == 1), the maximum amount is the amount in that house.

The robHelper function is a helper function that calculates the maximum amount of money that can be robbed using the same logic as the original House Robber problem. It takes a slice of integers nums and returns the maximum amount.

To solve House Robber II, we consider two cases:

Case 1: Rob the first house and exclude the last house. We pass nums[:len(nums)-1] to robHelper to calculate the maximum amount in this case.
Case 2: Exclude the first house and rob the last house. We pass nums[1:] to robHelper to calculate the maximum amount in this case.
Finally, we return the maximum amount from the two cases using the max function.

In the main function, we create an example scenario with nums = []int{2, 3, 2} representing the amount of money in each house arranged in a circular manner. We call the rob function with nums and print the maximum amount that can be robbed.

Please note that the code assumes valid input and non-negative amounts in the houses. Additional checks and error handling may be required based on specific requirements.


#### Longest Palindromic Substring

```
package main

import "fmt"

func longestPalindrome(s string) string {
	n := len(s)
	if n < 2 {
		return s
	}

	start, maxLength := 0, 1

	// Create a 2D slice to store the palindrome information
	dp := make([][]bool, n)
	for i := range dp {
		dp[i] = make([]bool, n)
	}

	// Every single character is a palindrome
	for i := 0; i < n; i++ {
		dp[i][i] = true
	}

	// Check for palindromes of length 2
	for i := 0; i < n-1; i++ {
		if s[i] == s[i+1] {
			dp[i][i+1] = true
			start = i
			maxLength = 2
		}
	}

	// Check for palindromes of length greater than 2
	for l := 3; l <= n; l++ {
		for i := 0; i <= n-l; i++ {
			j := i + l - 1
			if s[i] == s[j] && dp[i+1][j-1] {
				dp[i][j] = true
				start = i
				maxLength = l
			}
		}
	}

	return s[start : start+maxLength]
}

func main() {
	str := "babad"
	longest := longestPalindrome(str)
	fmt.Println("Longest palindromic substring:", longest)
}


```
In this code, the longestPalindrome function takes a string s and returns the longest palindromic substring. It uses a 1D dynamic programming approach to find the solution.

We first handle the base cases where if the length of the string is less than 2, the entire string is a palindrome and we can return it as the result.

We create a 2D slice dp to store the palindrome information. Each element dp[i][j] indicates whether the substring from index i to index j is a palindrome.

We iterate through the string to mark all the single characters as palindromes (dp[i][i] = true).

Next, we check for palindromes of length 2 by comparing adjacent characters (s[i] == s[i+1]). If we find a palindrome of length 2, we update the start and maxLength variables accordingly.

Then, we check for palindromes of length greater than 2 using a nested loop. We compare the characters at indices i and j and check if the substring from i+1 to j-1 is also a palindrome (dp[i+1][j-1]). If it is, we mark the current substring as a palindrome and update the start and maxLength variables.

Finally, we return the longest palindromic substring using the start and maxLength variables.

In the main function, we create an example string str and call the longestPalindrome function with str. We print the longest palindromic substring obtained.

Please note that the code assumes valid input. Additional checks and error handling may be required based on specific requirements.


#### Palindromic Substrings

```
package main

import "fmt"

func countSubstrings(s string) int {
	n := len(s)
	if n < 2 {
		return n
	}

	count := 0

	// Create a 2D slice to store the palindrome information
	dp := make([]bool, n)

	// Every single character is a palindrome
	for i := 0; i < n; i++ {
		dp[i] = true
		count++
	}

	// Check for palindromes of length 2
	for i := 0; i < n-1; i++ {
		if s[i] == s[i+1] {
			dp[i+1] = true
			count++
		}
	}

	// Check for palindromes of length greater than 2
	for l := 3; l <= n; l++ {
		for i := 0; i <= n-l; i++ {
			j := i + l - 1
			if s[i] == s[j] && dp[i+1] {
				dp[i] = true
				count++
			} else {
				dp[i] = false
			}
		}
	}

	return count
}

func main() {
	str := "abc"
	count := countSubstrings(str)
	fmt.Println("Number of palindromic substrings:", count)
}


```
In this code, the countSubstrings function takes a string s and returns the number of palindromic substrings in the given string. It uses a 1D dynamic programming approach to find the solution.

We first handle the base cases where if the length of the string is less than 2, the number of palindromic substrings is equal to the length of the string.

We create a 1D slice dp to store the palindrome information. Each element dp[i] indicates whether the substring from index 0 to index i is a palindrome.

We iterate through the string to mark all the single characters as palindromes (dp[i] = true) and increment the count variable.

Next, we check for palindromes of length 2 by comparing adjacent characters (s[i] == s[i+1]). If we find a palindrome of length 2, we mark dp[i+1] as true and increment the count variable.

Then, we check for palindromes of length greater than 2 using a nested loop. We compare the characters at indices i and j and check if the substring from i+1 to j-1 is also a palindrome (dp[i+1]). If it is, we mark dp[i] as true and increment the count variable. Otherwise, we mark dp[i] as false.

Finally, we return the count variable, which represents the total number of palindromic substrings in the given string.

In the main function, we create an example string str and call the countSubstrings function with str. We print the number of palindromic substrings obtained.

Please note that the code assumes valid input. Additional checks and error handling may be required based on specific requirements.


#### Decode Ways

```
package main

import (
	"fmt"
	"strconv"
)

func numDecodings(s string) int {
	n := len(s)
	if n == 0 || s[0] == '0' {
		return 0
	}

	// Create a 1D slice to store the number of decodings
	dp := make([]int, n+1)
	dp[0] = 1
	dp[1] = 1

	for i := 2; i <= n; i++ {
		// Check if the current character is a valid single-digit number
		if s[i-1] != '0' {
			dp[i] += dp[i-1]
		}

		// Check if the current and previous characters form a valid two-digit number
		if s[i-2] != '0' && (s[i-2]-'0')*10+(s[i-1]-'0') <= 26 {
			dp[i] += dp[i-2]
		}
	}

	return dp[n]
}

func main() {
	str := "226"
	decodings := numDecodings(str)
	fmt.Println("Number of decodings:", decodings)
}


```

In this code, the numDecodings function takes a string s and returns the number of possible decodings of the given string. It uses a 1D dynamic programming approach to find the solution.

We first handle the base cases where if the length of the string is 0 or the first character is '0', there are no valid decodings, so we return 0.

We create a 1D slice dp to store the number of decodings. Each element dp[i] represents the number of decodings for the substring from index 0 to index i.

We initialize dp[0] and dp[1] to 1, as there is only one possible decoding for an empty string or a single character string.

Then, we iterate from index 2 to n (length of the string). For each index i, we check two conditions:

If the current character s[i-1] is a valid single-digit number (not '0'), we add the number of decodings of the substring from index 0 to index i-1 (dp[i-1]) to dp[i].

If the current and previous characters form a valid two-digit number (between 10 and 26 inclusive), we add the number of decodings of the substring from index 0 to index i-2 (dp[i-2]) to dp[i].

Finally, we return dp[n], which represents the total number of possible decodings of the given string.

In the main function, we create an example string str and call the numDecodings function with str. We print the number of decodings obtained.

Please note that the code assumes valid input. Additional checks and error handling may be required based on specific requirements.


#### Coin Change

```
package main

import (
	"fmt"
	"math"
)

func coinChange(coins []int, amount int) int {
	// Initialize the 1D dynamic programming array with a maximum value
	dp := make([]int, amount+1)
	for i := range dp {
		dp[i] = math.MaxInt32
	}
	dp[0] = 0

	// Calculate the minimum number of coins needed for each amount
	for _, coin := range coins {
		for i := coin; i <= amount; i++ {
			dp[i] = min(dp[i], dp[i-coin]+1)
		}
	}

	if dp[amount] == math.MaxInt32 {
		return -1 // No combination of coins can make up the amount
	}

	return dp[amount]
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func main() {
	coins := []int{1, 2, 5}
	amount := 11
	minCoins := coinChange(coins, amount)
	fmt.Println("Minimum number of coins needed:", minCoins)
}


```

In this code, the coinChange function takes a slice of coins representing the available coin denominations and an integer amount representing the target amount. It returns the minimum number of coins needed to make up the target amount.

We first initialize a 1D dynamic programming array dp with a maximum value (math.MaxInt32) for each amount from 0 to amount. The index i of the array represents the amount, and dp[i] represents the minimum number of coins needed to make up that amount.

We set dp[0] to 0, as no coins are needed to make up the amount 0. For each coin denomination, we iterate from that coin value to the target amount. For each amount i, we calculate dp[i] by taking the minimum between its current value dp[i] and dp[i-coin]+1, where coin is the current coin denomination.

Finally, if the value at dp[amount] is still the maximum value, it means no combination of coins can make up the amount, so we return -1. Otherwise, we return the minimum number of coins needed, dp[amount].

In the main function, we create an example slice of coins with denominations [1, 2, 5] and set the target amount to 11. We call the coinChange function with these inputs and print the minimum number of coins needed.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.


#### Maximum Product Subarray

```
package main

import (
	"fmt"
	"math"
)

func maxProduct(nums []int) int {
	n := len(nums)
	if n == 0 {
		return 0
	}

	// Initialize the 1D dynamic programming array
	dpMin := make([]int, n) // Minimum product ending at index i
	dpMax := make([]int, n) // Maximum product ending at index i

	dpMin[0] = nums[0]
	dpMax[0] = nums[0]
	maxProduct := nums[0]

	for i := 1; i < n; i++ {
		// Update the minimum and maximum products ending at index i
		dpMin[i] = min(nums[i], min(nums[i]*dpMin[i-1], nums[i]*dpMax[i-1]))
		dpMax[i] = max(nums[i], max(nums[i]*dpMin[i-1], nums[i]*dpMax[i-1]))

		// Update the maximum product found so far
		maxProduct = max(maxProduct, dpMax[i])
	}

	return maxProduct
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	nums := []int{2, 3, -2, 4, -2}
	maxProd := maxProduct(nums)
	fmt.Println("Maximum product subarray:", maxProd)
}


```

In this code, the maxProduct function takes a slice of nums representing an array of integers. It returns the maximum product that can be obtained from a contiguous subarray within the input array.

We initialize two 1D dynamic programming arrays dpMin and dpMax, where dpMin[i] represents the minimum product ending at index i, and dpMax[i] represents the maximum product ending at index i.

We iterate through the input array starting from index 1 and update the dpMin and dpMax arrays based on the current element and the previous minimum and maximum products. We use the min and max helper functions to calculate the minimum and maximum products at each index.

After iterating through the entire array, we return the maximum product found in the dpMax array.

In the main function, we create an example slice of nums with values [2, 3, -2, 4, -2]. We call the maxProduct function with this input and print the maximum product subarray.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.


#### Word Break

```
package main

import (
	"fmt"
)

func wordBreak(s string, wordDict []string) bool {
	n := len(s)
	dp := make([]bool, n+1)
	dp[0] = true

	wordSet := make(map[string]bool)
	for _, word := range wordDict {
		wordSet[word] = true
	}

	for i := 1; i <= n; i++ {
		for j := 0; j < i; j++ {
			if dp[j] && wordSet[s[j:i]] {
				dp[i] = true
				break
			}
		}
	}

	return dp[n]
}

func main() {
	s := "leetcode"
	wordDict := []string{"leet", "code"}
	canBreak := wordBreak(s, wordDict)
	fmt.Println("Can break the word:", canBreak)
}


```

In this code, the wordBreak function takes a string s and a slice of wordDict representing a list of words. It returns a boolean value indicating whether the input string can be segmented into words from the dictionary.

We use the 1D dynamic programming approach to solve this problem. We create a boolean slice dp of size n+1, where n is the length of the input string s. The dp array is used to track if a substring of s from index 0 to i-1 can be segmented into words from the dictionary.

We initialize dp[0] as true to indicate that an empty string can be segmented. Then, for each index i from 1 to n, we iterate through the substring s[0:i] and check if there exists a word in the dictionary such that the prefix substring s[0:j] (where j is less than i) is also a valid segment. If we find such a word, we set dp[i] to true and break the inner loop.

After iterating through the entire string, the value of dp[n] represents whether the entire string s can be segmented into words.

In the main function, we create an example string s with value "leetcode" and a word dictionary wordDict with words "leet" and "code". We call the wordBreak function with these inputs and print whether the string can be broken into words.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.


#### Longest Increasing Subsequence




