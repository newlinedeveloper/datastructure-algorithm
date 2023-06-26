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




#### House Robber II


#### Longest Palindromic Substring


#### Palindromic Substrings


#### Decode Ways


#### Coin Change


#### Maximum Product Subarray


#### Word Break


#### Longest Increasing Subsequence




