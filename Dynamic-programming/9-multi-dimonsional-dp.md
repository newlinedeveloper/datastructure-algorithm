Certainly! Let's delve into the **State Machine / Multi-Dimensional DP** pattern, which is pivotal for problems involving multiple interdependent states.

---

## 🔍 How to Identify This Pattern

Look for problems that involve:

- **Multiple interdependent states**: e.g., holding/selling stocks, painting houses with color constraints.
- **State transitions**: Decisions at one step affect future options.
- **Constraints**: Such as cooldown periods, limited transactions, or adjacent restrictions.
- **Optimization goals**: Maximizing profit, minimizing cost, etc.

---

## 🟢 1. Best Time to Buy and Sell Stock II  
**LeetCode #122 – Easy**  
[Problem Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

### ✅ Problem Statement

Given an array `prices` where `prices[i]` is the price of a given stock on the `iᵗʰ` day, you can complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

### 🧠 Approach

This problem allows for multiple transactions with the constraint that you must sell before buying again. The optimal strategy is to capture all upward price movements.

### ✅ Golang Code

```go
func maxProfit(prices []int) int {
    profit := 0
    for i := 1; i < len(prices); i++ {
        if prices[i] > prices[i-1] {
            profit += prices[i] - prices[i-1]
        }
    }
    return profit
}
```

### 🧪 Sample Input/Output

**Input:** `[7,1,5,3,6,4]`  
**Output:** `7`  
**Explanation:** Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 4. Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 3. Total profit = 7.

---

## 🟢 2. Paint House  
**LeetCode #256 – Medium**  
[Problem Link](https://leetcode.com/problems/paint-house/)

### ✅ Problem Statement

There is a row of `n` houses, each can be painted red, blue, or green. The cost of painting each house with a certain color is different. No two adjacent houses can have the same color. Return the minimum cost to paint all houses.

### 🧠 Approach

Use dynamic programming to track the minimum cost of painting each house with each color, considering the constraint that no two adjacent houses can have the same color.

### ✅ Golang Code

```go
func minCost(costs [][]int) int {
    if len(costs) == 0 {
        return 0
    }

    for i := 1; i < len(costs); i++ {
        costs[i][0] += min(costs[i-1][1], costs[i-1][2])
        costs[i][1] += min(costs[i-1][0], costs[i-1][2])
        costs[i][2] += min(costs[i-1][0], costs[i-1][1])
    }

    n := len(costs)
    return min(costs[n-1][0], min(costs[n-1][1], costs[n-1][2]))
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### 🧪 Sample Input/Output

**Input:** `[[17,2,17],[16,16,5],[14,3,19]]`  
**Output:** `10`  
**Explanation:** Paint house 0 into blue, house 1 into green, and house 2 into blue. Minimum cost: 2 + 5 + 3 = 10.

---
Certainly! Let's continue exploring the **State Machine / Multi-Dimensional DP** pattern with the remaining problems:

---

## 🟡 3. Best Time to Buy and Sell Stock with Cooldown  
**LeetCode #309 – Medium**  
[Problem Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

### ✅ Problem Statement

Given an array `prices` where `prices[i]` is the price of a given stock on the `iᵗʰ` day, design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
- You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

### 🧠 Approach

This problem introduces a cooldown period after selling a stock. To model this, we use three states:

- **hold**: Maximum profit when holding a stock.
- **sold**: Maximum profit when having just sold a stock.
- **rest**: Maximum profit when in cooldown or doing nothing.

We update these states as we iterate through the prices.

### ✅ Golang Code

```go
func maxProfit(prices []int) int {
    if len(prices) == 0 {
        return 0
    }
    n := len(prices)
    hold := make([]int, n)
    sold := make([]int, n)
    rest := make([]int, n)

    hold[0] = -prices[0]
    sold[0] = 0
    rest[0] = 0

    for i := 1; i < n; i++ {
        hold[i] = max(hold[i-1], rest[i-1]-prices[i])
        sold[i] = hold[i-1] + prices[i]
        rest[i] = max(rest[i-1], sold[i-1])
    }

    return max(sold[n-1], rest[n-1])
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### 🧪 Sample Input/Output

**Input:** `[1,2,3,0,2]`  
**Output:** `3`  
**Explanation:** Transactions = [buy, sell, cooldown, buy, sell] → Profit = 3

---

## 🔴 4. Best Time to Buy and Sell Stock III (2 transactions)  
**LeetCode #123 – Hard**  
[Problem Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

### ✅ Problem Statement

You are given an array `prices` where `prices[i]` is the price of a given stock on the `iᵗʰ` day. Find the maximum profit you can achieve. You may complete at most two transactions.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

### 🧠 Approach

We can model this problem using dynamic programming by tracking four variables:

- **buy1**: Maximum profit after the first buy.
- **sell1**: Maximum profit after the first sell.
- **buy2**: Maximum profit after the second buy.
- **sell2**: Maximum profit after the second sell.

We iterate through the prices and update these variables accordingly.

### ✅ Golang Code

```go
func maxProfit(prices []int) int {
    buy1, buy2 := math.MinInt32, math.MinInt32
    sell1, sell2 := 0, 0

    for _, price := range prices {
        buy1 = max(buy1, -price)
        sell1 = max(sell1, buy1+price)
        buy2 = max(buy2, sell1-price)
        sell2 = max(sell2, buy2+price)
    }

    return sell2
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### 🧪 Sample Input/Output

**Input:** `[3,3,5,0,0,3,1,4]`  
**Output:** `6`  
**Explanation:** Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3. Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 3. Total profit = 6.

---

## 🟣 5. Dungeon Game  
**LeetCode #174 – Hard**  
[Problem Link](https://leetcode.com/problems/dungeon-game/)

### ✅ Problem Statement

The knight has to rescue the princess by moving from the top-left corner to the bottom-right corner of a dungeon grid. Each cell contains an integer representing the health points gained or lost. The knight can only move right or down. Determine the minimum initial health required for the knight to reach the princess alive.

### 🧠 Approach

We use dynamic programming starting from the bottom-right cell and moving backwards. At each cell, we calculate the minimum health needed to move to the next cell, ensuring the knight's health never drops below 1.

### ✅ Golang Code

```go
func calculateMinimumHP(dungeon [][]int) int {
    m, n := len(dungeon), len(dungeon[0])
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
        for j := range dp[i] {
            dp[i][j] = math.MaxInt32
        }
    }
    dp[m][n-1], dp[m-1][n] = 1, 1

    for i := m - 1; i >= 0; i-- {
        for j := n - 1; j >= 0; j-- {
            minHP := min(dp[i+1][j], dp[i][j+1]) - dungeon[i][j]
            dp[i][j] = max(1, minHP)
        }
    }

    return dp[0][0]
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
```

### 🧪 Sample Input/Output

**Input:** `[[-2,-3,3],[-5,-10,1],[10,30,-5]]`  
**Output:** `7`  
**Explanation:** The knight needs at least 7 health points to reach the princess alive.

---

Here’s the continuation and completion of the **State Machine / Multi-Dimensional DP** pattern with the **Traveling Salesman Problem (TSP)** using bitmasking and dynamic programming:

---

## 🟤 6. Traveling Salesman Problem (TSP)  
**GeeksForGeeks – Hard**  
🔗 [Problem Link](https://www.geeksforgeeks.org/travelling-salesman-problem-set-1/)

### ✅ Problem Statement

Given a set of `n` cities and the distance between every pair of cities, find the shortest possible route that visits each city exactly once and returns to the origin city.

### 🔍 Identifying the Pattern

TSP is a classic **state machine** problem:
- The **state** includes the current city and a bitmask of visited cities.
- The **transition** is to go from the current city to an unvisited city.
- We aim to **minimize the total travel distance**.

This is a textbook use case of **multi-dimensional DP** (city, visited subset).

---

### 🧠 Approach

Let `dp[mask][u]` be the minimum cost to visit all cities in `mask` ending at city `u`.

Base Case:  
- `dp[1<<0][0] = 0` → Start at city `0` with only city `0` visited.

Transition:
- For each city `v` not in `mask`, update:  
  `dp[mask | (1<<v)][v] = min(dp[mask | (1<<v)][v], dp[mask][u] + cost[u][v])`

Result:
- `min(dp[ALL_VISITED][u] + cost[u][0])` for all `u`

---

### ✅ Golang Code

```go
import (
    "math"
)

func tsp(cost [][]int) int {
    n := len(cost)
    dp := make([][]int, 1<<n)
    for i := range dp {
        dp[i] = make([]int, n)
        for j := range dp[i] {
            dp[i][j] = math.MaxInt32
        }
    }

    dp[1][0] = 0 // start from city 0

    for mask := 1; mask < (1 << n); mask++ {
        for u := 0; u < n; u++ {
            if mask&(1<<u) == 0 {
                continue
            }
            for v := 0; v < n; v++ {
                if mask&(1<<v) == 0 {
                    next := mask | (1 << v)
                    dp[next][v] = min(dp[next][v], dp[mask][u]+cost[u][v])
                }
            }
        }
    }

    res := math.MaxInt32
    for u := 1; u < n; u++ {
        res = min(res, dp[(1<<n)-1][u]+cost[u][0])
    }
    return res
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

---

### 🧪 Sample Input/Output

```go
cost := [][]int{
    {0, 10, 15, 20},
    {10, 0, 35, 25},
    {15, 35, 0, 30},
    {20, 25, 30, 0},
}
fmt.Println(tsp(cost)) // Output: 80
```

**Explanation:**
- Best tour: 0 → 1 → 3 → 2 → 0 → total cost = 10 + 25 + 30 + 15 = **80**

---

### ✅ How to Identify This Pattern

This pattern usually includes:
- A **grid, graph, or list** of decisions (days, colors, cities).
- A **state** that includes multiple variables (e.g., `day`, `holding/not`, `transactions`, `visited mask`).
- The problem asks for **min/max result** considering transitions between these states.

---
