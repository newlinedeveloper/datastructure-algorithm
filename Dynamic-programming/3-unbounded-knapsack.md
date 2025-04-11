Awesome! You're diving into the **Unbounded Knapsack Pattern** — another fundamental dynamic programming pattern, often confused with 0/1 Knapsack but critically different. Let's break it all down with **clues, Golang code, clear logic, and sample I/O** for each.

---

## ✅ 3. Unbounded Knapsack Pattern

---

### 🔍 **How to Identify This Pattern**

Use this pattern when:
| Clue in Problem Statement                        | Indicates                           |
|--------------------------------------------------|-------------------------------------|
| "You can use the same item multiple times"       | Unbounded (unlimited use per item)  |
| "Infinite supply of coins/items"                 | Reuse allowed                       |
| "Minimum number of items to achieve a value"     | Optimization with repetition        |
| "Maximum value with rod cuts of any length"      | Similar to unbounded knapsack       |

---

### 🧠 Key Recurrence

For item `i` and capacity `j`:
```
dp[i][j] = max(dp[i-1][j], dp[i][j - wt[i]] + val[i])
```
> Notice: use `dp[i][j - wt[i]]` instead of `dp[i-1][...]` — allows **reusing** the item.

---

## 🟢 1. Coin Change (Minimum Coins) — [LeetCode #322](https://leetcode.com/problems/coin-change/)

### 🔸 Problem:
You’re given coins `[1, 2, 5]` and amount `11`. Return the **minimum number of coins** to make the amount.

---

### ✅ Golang Code:

```go
func coinChange(coins []int, amount int) int {
    dp := make([]int, amount+1)
    for i := 1; i <= amount; i++ {
        dp[i] = amount + 1
    }
    dp[0] = 0

    for _, coin := range coins {
        for j := coin; j <= amount; j++ {
            dp[j] = min(dp[j], dp[j-coin]+1)
        }
    }

    if dp[amount] > amount {
        return -1
    }
    return dp[amount]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

---

### 💡 Sample Input / Output:

**Input:** `coins = [1, 2, 5], amount = 11`  
**Output:** `3`  
**Explanation:** `5 + 5 + 1 = 11`

---

## 🟢 2. Coin Change II (Count Ways) — [LeetCode #518](https://leetcode.com/problems/coin-change-ii/)

### 🔸 Problem:
Return the number of combinations to make up the amount.

---

### ✅ Golang Code:

```go
func change(amount int, coins []int) int {
    dp := make([]int, amount+1)
    dp[0] = 1

    for _, coin := range coins {
        for j := coin; j <= amount; j++ {
            dp[j] += dp[j - coin]
        }
    }
    return dp[amount]
}
```

---

### 💡 Sample Input / Output:

**Input:** `coins = [1, 2, 5], amount = 5`  
**Output:** `4`  
**Explanation:** The combinations are `[5], [2+2+1], [2+1+1+1], [1+1+1+1+1]`

---

## 🟢 3. Rod Cutting — [GeeksForGeeks](https://www.geeksforgeeks.org/cutting-a-rod-dp-13/)

### 🔸 Problem:
You are given a rod of length `n` and a price array. Cut the rod to maximize the price.

---

### ✅ Golang Code:

```go
func cutRod(price []int, n int) int {
    dp := make([]int, n+1)

    for i := 1; i <= n; i++ {
        for j := 0; j < i; j++ {
            dp[i] = max(dp[i], price[j] + dp[i-j-1])
        }
    }

    return dp[n]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

---

### 💡 Sample Input / Output:

**Input:** `price = [1, 5, 8, 9], n = 4`  
**Output:** `10`  
**Explanation:** Best cut: 2 + 2 (price 5 + 5)

---

## 🟢 4. Minimum Cost to Reach Destination — [GFG](https://www.geeksforgeeks.org/min-cost-path-dp-6/)

### 🔸 Problem:
Given ticket costs and distances, find the **minimum cost to reach a station**.

---

This is often modeled similar to coin change or unbounded knapsack.

---

### ✅ Golang Code:

```go
func minCostToDestination(costs []int, distances []int, totalDistance int) int {
    dp := make([]int, totalDistance+1)
    for i := 1; i <= totalDistance; i++ {
        dp[i] = 1<<30 // simulate infinity
        for j, d := range distances {
            if i >= d {
                dp[i] = min(dp[i], dp[i-d] + costs[j])
            }
        }
    }
    return dp[totalDistance]
}
```

---

### 💡 Sample Input / Output:

**Input:**  
- `distances = [1, 3, 5]`  
- `costs = [1, 4, 7]`  
- `totalDistance = 7`  
**Output:** `6`  
**Explanation:** Use ticket of 1 twice and 5 once (1+1+4)

---

## 🔁 Summary Table

| Problem                        | Goal                   | Key Idea                    |
|-------------------------------|------------------------|-----------------------------|
| Coin Change                   | Min coins              | DP from coin to amount      |
| Coin Change II                | Count combinations     | Reuse allowed, 1D DP        |
| Rod Cutting                   | Max revenue            | Try all cuts, reuse allowed |
| Min Cost to Destination       | Min cost               | Generalized coin change     |

---
