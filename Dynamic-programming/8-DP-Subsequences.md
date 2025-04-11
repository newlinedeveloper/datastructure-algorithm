 **DP on Subsequences** 
---

## 🎯 8. DP on Subsequences — Identify patterns in subarrays or subsequences

---

### 🔍 How to Identify This Pattern

| Clue in Problem | Interpretation |
|----------------|----------------|
| Keywords: "longest", "maximum sum", "number of subsequences" | Usually means DP on subsequences |
| You need to skip elements or make choices to build a sequence | Track previous results |
| The result may or may not include all elements | Use nested loops or binary search |

---

## 🟢 1. Longest Increasing Subsequence (LIS)  
**LeetCode #300**  
[🔗 Problem Link](https://leetcode.com/problems/longest-increasing-subsequence)

---

### ✅ Problem:
> Return the **length** of the **longest strictly increasing subsequence** in the array.

---

### ✅ Golang Code:

```go
func lengthOfLIS(nums []int) int {
    n := len(nums)
    dp := make([]int, n)
    maxLen := 1

    for i := 0; i < n; i++ {
        dp[i] = 1 // Every element is a subsequence
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                dp[i] = max(dp[i], dp[j]+1)
            }
        }
        maxLen = max(maxLen, dp[i])
    }
    return maxLen
}

func max(a, b int) int {
    if a > b { return a }
    return b
}
```

---

### 🧪 Sample I/O

**Input:** `[10, 9, 2, 5, 3, 7, 101, 18]`  
**Output:** `4`  
**Explanation:** LIS = `[2, 3, 7, 101]`

---

## 🟢 2. Number of Longest Increasing Subsequences  
**LeetCode #673**  
[🔗 Problem Link](https://leetcode.com/problems/number-of-longest-increasing-subsequence)

---

### ✅ Problem:
> Return the **number of LIS subsequences** in the array.

---

### ✅ Golang Code:

```go
func findNumberOfLIS(nums []int) int {
    n := len(nums)
    dp := make([]int, n)
    count := make([]int, n)
    maxLen, res := 0, 0

    for i := 0; i < n; i++ {
        dp[i], count[i] = 1, 1
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                if dp[j]+1 > dp[i] {
                    dp[i] = dp[j] + 1
                    count[i] = count[j]
                } else if dp[j]+1 == dp[i] {
                    count[i] += count[j]
                }
            }
        }

        if dp[i] > maxLen {
            maxLen = dp[i]
            res = count[i]
        } else if dp[i] == maxLen {
            res += count[i]
        }
    }

    return res
}
```

---

### 🧪 Sample I/O

**Input:** `[1,3,5,4,7]`  
**Output:** `2`  
**Explanation:** Two LIS: `[1,3,4,7]` and `[1,3,5,7]`

---

## 🟢 3. Maximum Sum Increasing Subsequence  
**GeeksForGeeks**  
[🔗 Problem Link](https://www.geeksforgeeks.org/maximum-sum-increasing-subsequence-dp-14/)

---

### ✅ Problem:
> Find the maximum sum of an increasing subsequence in the array.

---

### ✅ Golang Code:

```go
func maxSumIS(nums []int) int {
    n := len(nums)
    dp := make([]int, n)
    maxSum := 0

    for i := 0; i < n; i++ {
        dp[i] = nums[i]
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                dp[i] = max(dp[i], dp[j]+nums[i])
            }
        }
        maxSum = max(maxSum, dp[i])
    }

    return maxSum
}
```

---

### 🧪 Sample I/O

**Input:** `[1, 101, 2, 3, 100, 4, 5]`  
**Output:** `106`  
**Explanation:** Max sum LIS = `[1, 2, 3, 100]`

---

## 🔴 4. Russian Doll Envelopes  
**LeetCode #354**  
[🔗 Problem Link](https://leetcode.com/problems/russian-doll-envelopes)

---

### ✅ Problem:
> Each envelope has width and height. One envelope can fit into another if both width and height are greater. Return the maximum number of envelopes you can nest.

---

### ✅ Golang Code (Optimized with Binary Search):

```go
import "sort"

func maxEnvelopes(envelopes [][]int) int {
    sort.Slice(envelopes, func(i, j int) bool {
        if envelopes[i][0] == envelopes[j][0] {
            return envelopes[i][1] > envelopes[j][1]
        }
        return envelopes[i][0] < envelopes[j][0]
    })

    heights := []int{}
    for _, env := range envelopes {
        h := env[1]
        idx := sort.Search(len(heights), func(i int) bool {
            return heights[i] >= h
        })
        if idx == len(heights) {
            heights = append(heights, h)
        } else {
            heights[idx] = h
        }
    }

    return len(heights)
}
```

---

### 🧪 Sample I/O

**Input:** `[[5,4],[6,4],[6,7],[2,3]]`  
**Output:** `3`  
**Explanation:** Nesting possible: `[2,3] => [5,4] => [6,7]`

---

## 🧠 Summary Table

| Problem | Goal | Key Idea | Time Complexity |
|--------|------|----------|-----------------|
| LIS | Max length | DP[i] = max(DP[j]+1) | O(n²) |
| # LIS | Count LIS | Track length and count arrays | O(n²) |
| Max Sum IS | Max sum | Replace count with sum | O(n²) |
| Russian Dolls | Longest nesting | Sort + LIS on height | O(n log n) |

---
