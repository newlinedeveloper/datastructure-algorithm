### Dynamic Programming Patterns and Solutions in Go

---

#### 1. Climbing Stairs (Fibonacci Pattern)
**Pattern**: Fibonacci / Recurrence
```go
func climbStairs(n int) int {
    if n <= 2 {
        return n
    }
    first, second := 1, 2
    for i := 3; i <= n; i++ {
        first, second = second, first+second
    }
    return second
}
```
**Input**: `n = 5`
**Output**: `8`

---

#### 2. Longest Common Subsequence (LCS Pattern)
**Pattern**: Longest Common Subsequence
```go
func longestCommonSubsequence(text1 string, text2 string) int {
    m, n := len(text1), len(text2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }

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
```
**Input**: `text1 = "abcde", text2 = "ace"`
**Output**: `3`

---

#### 3. Word Break (String Segmentation Pattern)
**Pattern**: Word Break / String Segmentation
```go
func wordBreak(s string, wordDict []string) bool {
    wordSet := make(map[string]bool)
    for _, word := range wordDict {
        wordSet[word] = true
    }
    dp := make([]bool, len(s)+1)
    dp[0] = true

    for i := 1; i <= len(s); i++ {
        for j := 0; j < i; j++ {
            if dp[j] && wordSet[s[j:i]] {
                dp[i] = true
                break
            }
        }
    }
    return dp[len(s)]
}
```
**Input**: `s = "leetcode", wordDict = ["leet", "code"]`
**Output**: `true`

---

#### 4. House Robber (DP on Arrays)
**Pattern**: DP on Linear Array
```go
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
```
**Input**: `nums = [2,7,9,3,1]`
**Output**: `12`

---

#### 5. Unique Paths (Matrix DP Pattern)
**Pattern**: Matrix DP
```go
func uniquePaths(m int, n int) int {
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
        dp[i][0] = 1
    }
    for j := 0; j < n; j++ {
        dp[0][j] = 1
    }
    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
        }
    }
    return dp[m-1][n-1]
}
```
**Input**: `m = 3, n = 7`
**Output**: `28`

---

#### 6. Decode Ways (Fibonacci Variant)
**Pattern**: Fibonacci / Recurrence
```go
func numDecodings(s string) int {
    if len(s) == 0 || s[0] == '0' {
        return 0
    }
    dp := make([]int, len(s)+1)
    dp[0], dp[1] = 1, 1

    for i := 2; i <= len(s); i++ {
        oneDigit := s[i-1] - '0'
        twoDigit := (s[i-2]-'0')*10 + (s[i-1] - '0')
        if oneDigit > 0 {
            dp[i] += dp[i-1]
        }
        if twoDigit >= 10 && twoDigit <= 26 {
            dp[i] += dp[i-2]
        }
    }
    return dp[len(s)]
}
```
**Input**: `s = "226"`
**Output**: `3`

---

#### 7. Jump Game (Greedy or DP)
**Pattern**: DP / Greedy
```go
func canJump(nums []int) bool {
    maxReach := 0
    for i, num := range nums {
        if i > maxReach {
            return false
        }
        maxReach = max(maxReach, i+num)
    }
    return true
}
```
**Input**: `nums = [2,3,1,1,4]`
**Output**: `true`

---

#### 8. Partition Equal Subset Sum (0/1 Knapsack Pattern)
**Pattern**: 0/1 Knapsack
```go
func canPartition(nums []int) bool {
    sum := 0
    for _, num := range nums {
        sum += num
    }
    if sum%2 != 0 {
        return false
    }
    target := sum / 2
    dp := make([]bool, target+1)
    dp[0] = true

    for _, num := range nums {
        for j := target; j >= num; j-- {
            dp[j] = dp[j] || dp[j-num]
        }
    }
    return dp[target]
}
```
**Input**: `nums = [1,5,11,5]`
**Output**: `true`

---

#### 9. Maximum Subarray (Kadane’s Algorithm)
**Pattern**: DP / Greedy
```go
func maxSubArray(nums []int) int {
    maxSum := nums[0]
    currSum := nums[0]
    for i := 1; i < len(nums); i++ {
        currSum = max(nums[i], currSum+nums[i])
        maxSum = max(maxSum, currSum)
    }
    return maxSum
}
```
**Input**: `nums = [-2,1,-3,4,-1,2,1,-5,4]`
**Output**: `6`

---

#### 10. Longest Increasing Subsequence (DP on Subsequences)
**Pattern**: DP on Subsequences
```go
func lengthOfLIS(nums []int) int {
    dp := make([]int, len(nums))
    for i := range dp {
        dp[i] = 1
    }
    maxLen := 1
    for i := 1; i < len(nums); i++ {
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                dp[i] = max(dp[i], dp[j]+1)
            }
        }
        maxLen = max(maxLen, dp[i])
    }
    return maxLen
}
```
**Input**: `nums = [10,9,2,5,3,7,101,18]`
**Output**: `4`
