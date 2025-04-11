Great! Let's explore the **0/1 Knapsack Pattern** — one of the most frequently asked dynamic programming patterns in **interviews at companies like Google, Amazon, and Meta**. You're smart to master it deeply, especially with real-world-style problems like these.

---

## ✅ **2. 0/1 Knapsack Pattern**

---

### 🔍 **How to Identify This Pattern**

You'll likely use the **0/1 Knapsack pattern** when:

| Clue in Problem Statement | Meaning |
|---------------------------|---------|
| "Pick or not pick" / "Include or exclude" | Binary choice for each item |
| You're given a `capacity`, `weight`, or `target` | A constraint limiting selections |
| Maximize, minimize, or count combinations | Optimization or enumeration |
| You're looping through elements and trying combinations | Subsets and combinations |

---

## 🔁 Pattern Summary

| Concept              | Description |
|----------------------|-------------|
| Type                 | Binary decision (take or skip) |
| Recurrence relation  | `dp[i][j] = max(dp[i-1][j], dp[i-1][j-weight[i]] + value[i])` |
| Base case            | Zero capacity or no items = 0 |
| Optimization         | Often compress to 1D DP |
| Problem class        | Subset sum, target sum, resource allocation |

---

Now, let’s tackle each problem 👇

---

### 🟢 **1. Subset Sum** — *GFG / LeetCode Discuss*

#### 🔸 Problem:
Given a set of positive integers and a value `sum`, check if there's a subset with a total sum equal to `sum`.

#### ✅ Golang Code:

```go
func isSubsetSum(arr []int, sum int) bool {
    n := len(arr)
    dp := make([][]bool, n+1)
    for i := range dp {
        dp[i] = make([]bool, sum+1)
        dp[i][0] = true
    }

    for i := 1; i <= n; i++ {
        for j := 1; j <= sum; j++ {
            if arr[i-1] <= j {
                dp[i][j] = dp[i-1][j] || dp[i-1][j-arr[i-1]]
            } else {
                dp[i][j] = dp[i-1][j]
            }
        }
    }
    return dp[n][sum]
}
```

#### 💡 Explanation:
- For each number, we decide: include or not include.
- `dp[i][j]` = is it possible to form sum `j` using the first `i` elements.

---

### 🟢 **2. Partition Equal Subset Sum** — [LeetCode #416](https://leetcode.com/problems/partition-equal-subset-sum/)

#### 🔸 Problem:
Can the array be partitioned into two subsets with equal sum?

#### ✅ Golang Code:

```go
func canPartition(nums []int) bool {
    total := 0
    for _, num := range nums {
        total += num
    }
    if total%2 != 0 {
        return false
    }
    target := total / 2

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

#### 💡 Explanation:
- Classic **subset sum** where target = total/2.
- Use 1D space-optimized array: from right to left to avoid overwriting.

---

### 🟢 **3. Target Sum** — [LeetCode #494](https://leetcode.com/problems/target-sum/)

#### 🔸 Problem:
Given an array and a target, assign '+' or '-' to each number to reach the target. Return number of ways.

#### ✅ Golang Code:

```go
func findTargetSumWays(nums []int, target int) int {
    sum := 0
    for _, num := range nums {
        sum += num
    }

    if (sum+target)%2 != 0 || abs(target) > sum {
        return 0
    }

    return countSubsets(nums, (sum+target)/2)
}

func countSubsets(nums []int, s int) int {
    dp := make([]int, s+1)
    dp[0] = 1

    for _, num := range nums {
        for j := s; j >= num; j-- {
            dp[j] += dp[j-num]
        }
    }
    return dp[s]
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

#### 💡 Explanation:
- Converts to **subset sum**: `target = (sum + target) / 2`
- Why? Because you are effectively dividing the array into + and - subsets.

---

### 🟢 **4. 0/1 Knapsack** — *HackerRank*

#### 🔸 Problem:
Given weights and values of items and capacity `W`, find max value you can carry.

#### ✅ Golang Code:

```go
func knapsack(wt []int, val []int, W int) int {
    n := len(wt)
    dp := make([][]int, n+1)
    for i := range dp {
        dp[i] = make([]int, W+1)
    }

    for i := 1; i <= n; i++ {
        for j := 1; j <= W; j++ {
            if wt[i-1] <= j {
                dp[i][j] = max(dp[i-1][j], dp[i-1][j-wt[i-1]]+val[i-1])
            } else {
                dp[i][j] = dp[i-1][j]
            }
        }
    }
    return dp[n][W]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#### 💡 Explanation:
- Either include the item (and subtract its weight) or skip it.
- Build up with 2D DP table based on `items` and `capacity`.

---

### 🟢 **5. Count of Subset Sum** — *GeeksForGeeks*

#### 🔸 Problem:
Count number of subsets that sum to a target.

#### ✅ Golang Code:

```go
func countSubsetsWithSum(arr []int, sum int) int {
    n := len(arr)
    dp := make([][]int, n+1)
    for i := range dp {
        dp[i] = make([]int, sum+1)
        dp[i][0] = 1
    }

    for i := 1; i <= n; i++ {
        for j := 0; j <= sum; j++ {
            if arr[i-1] <= j {
                dp[i][j] = dp[i-1][j] + dp[i-1][j-arr[i-1]]
            } else {
                dp[i][j] = dp[i-1][j]
            }
        }
    }
    return dp[n][sum]
}
```

#### 💡 Explanation:
- For each element, either use it or skip it.
- Instead of max value, we are **counting combinations**.

---

## ✨ Recap Table

| Problem                          | Type        | Goal                     | Technique          |
|----------------------------------|-------------|--------------------------|--------------------|
| Subset Sum                       | Feasibility | Can we reach sum?        | 2D DP table        |
| Partition Equal Subset Sum       | Feasibility | Can we split equally?    | 1D DP Optimization |
| Target Sum                       | Count       | How many expressions?    | Transform to Subset|
| 0/1 Knapsack                     | Optimization| Max value within capacity| 2D DP              |
| Count of Subset Sum              | Count       | How many subsets match?  | 2D DP              |

---

