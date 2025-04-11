Absolutely! Let’s break down the **Fibonacci / Recurrence Pattern** in detail.

---

## ✅ **1. Understanding the Fibonacci / Recurrence Pattern**

### 🔍 **How to Identify This Pattern**
You’re likely dealing with a Fibonacci-style DP problem if:
- The solution to the problem depends **only on a fixed number of previous results** (e.g., `dp[i] = dp[i-1] + dp[i-2]`)
- There’s a **single path to a result**, usually linear.
- You can define a **clear recurrence relation**.
- The problem can be solved **top-down with memoization** or **bottom-up with tabulation**.

---

## 🔁 Pattern Summary
| Feature                | Description                                  |
|------------------------|----------------------------------------------|
| Recurrence Relation    | `dp[i] = dp[i-1] + dp[i-2]` (or similar)      |
| Base Cases             | Usually `dp[0]`, `dp[1]`                      |
| Optimization           | Can be done using just 2 or 3 variables       |
| Typical Usage          | Counting paths, sequence values              |

---

Now, let's dive into the examples 👇

---

### 🟢 **1. Fibonacci Number** — [LeetCode #509](https://leetcode.com/problems/fibonacci-number/)

#### 🔸 Problem:
Compute the `n-th` Fibonacci number where:
```
F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2) for n > 1
```

#### ✅ Golang Code:

```go
func fib(n int) int {
    if n <= 1 {
        return n
    }
    a, b := 0, 1
    for i := 2; i <= n; i++ {
        a, b = b, a+b
    }
    return b
}
```

#### 💡 Explanation:
- We only need the last two values to compute the next one.
- Space optimization: we use variables instead of a full array.

---

### 🟢 **2. Climbing Stairs** — [LeetCode #70](https://leetcode.com/problems/climbing-stairs/)

#### 🔸 Problem:
You can climb either 1 or 2 steps. In how many distinct ways can you reach the top (step `n`)?

#### ✅ Golang Code:

```go
func climbStairs(n int) int {
    if n <= 2 {
        return n
    }
    a, b := 1, 2
    for i := 3; i <= n; i++ {
        a, b = b, a + b
    }
    return b
}
```

#### 💡 Explanation:
- This is **identical to Fibonacci** logic.
- Base cases: `1 step = 1 way`, `2 steps = 2 ways`
- Each step `i` depends on `i-1` and `i-2`.

---

### 🟢 **3. Tribonacci Number** — [LeetCode #1137](https://leetcode.com/problems/n-th-tribonacci-number/)

#### 🔸 Problem:
```
T(0) = 0, T(1) = 1, T(2) = 1
T(n) = T(n-1) + T(n-2) + T(n-3)
```

#### ✅ Golang Code:

```go
func tribonacci(n int) int {
    if n == 0 {
        return 0
    } else if n <= 2 {
        return 1
    }

    a, b, c := 0, 1, 1
    for i := 3; i <= n; i++ {
        a, b, c = b, c, a+b+c
    }
    return c
}
```

#### 💡 Explanation:
- It’s a generalized Fibonacci using **3 previous values**.

---

### 🟡 **4. Number of Ways to Reach the End** — *HackerRank (or similar problems)*

#### 🔸 Problem:
You’re on the first cell of an array, and each element `arr[i]` tells how far you can jump. Find how many distinct ways to reach the last cell.

#### ✅ Golang Code:

```go
func countWays(arr []int) int {
    n := len(arr)
    dp := make([]int, n)
    dp[0] = 1

    for i := 0; i < n; i++ {
        for j := 1; j <= arr[i] && i+j < n; j++ {
            dp[i+j] += dp[i]
        }
    }
    return dp[n-1]
}
```

#### 💡 Explanation:
- Each index can "spread" its value to the reachable future indices.
- `dp[i]` = number of ways to reach index `i`.

---

### 🟡 **5. Decode Ways** — [LeetCode #91](https://leetcode.com/problems/decode-ways/)

#### 🔸 Problem:
A digit string represents letters (e.g. '1' = A, '26' = Z). How many ways can you decode the string?

#### ✅ Golang Code:

```go
func numDecodings(s string) int {
    n := len(s)
    if s[0] == '0' {
        return 0
    }

    dp := make([]int, n+1)
    dp[0], dp[1] = 1, 1

    for i := 2; i <= n; i++ {
        one := s[i-1 : i]
        two := s[i-2 : i]

        if one >= "1" && one <= "9" {
            dp[i] += dp[i-1]
        }
        if two >= "10" && two <= "26" {
            dp[i] += dp[i-2]
        }
    }

    return dp[n]
}
```

#### 💡 Explanation:
- `dp[i]` = ways to decode up to character `i`
- You can decode:
  - One digit if it's 1-9
  - Two digits if it's 10-26
- Recurrence: `dp[i] = dp[i-1] + dp[i-2]`

---

## 📌 Final Notes

| Problem                | Recurrence Relation Example                  |
|------------------------|----------------------------------------------|
| Fibonacci              | `dp[i] = dp[i-1] + dp[i-2]`                  |
| Climbing Stairs        | `dp[i] = dp[i-1] + dp[i-2]`                  |
| Tribonacci             | `dp[i] = dp[i-1] + dp[i-2] + dp[i-3]`        |
| Ways to Reach End      | `dp[i+j] += dp[i]` for all valid `j`         |
| Decode Ways            | `dp[i] = dp[i-1] (if valid) + dp[i-2] (if valid)` |

---
