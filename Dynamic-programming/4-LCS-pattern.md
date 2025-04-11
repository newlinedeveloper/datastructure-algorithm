Great! Let's dive into the **Longest Common Subsequence (LCS) Pattern** — one of the most foundational patterns in dynamic programming that’s especially common in string manipulation problems and real-world applications like **diff tools**, **DNA sequence alignment**, and **text similarity**.

---

## 🧩 4. Longest Common Subsequence (LCS) Pattern

---

### 🔍 **How to Identify This Pattern**

You'll likely use this pattern when:

| Problem Clues | What It Hints |
|---------------|---------------|
| “Find the longest subsequence…” | Look for LCS-style recurrence |
| “Minimum insertions/deletions to make two strings equal” | Use LCS or a variant |
| “Convert one string to another (insert/delete/replace)” | Use Edit Distance (variant) |
| “Palindrome from string” | Apply LCS logic on `s` and `reverse(s)` |
| “Supersequence or Subsequence logic” | Often built on LCS or its length |

---

### 📐 Core LCS Recurrence

For strings `s1` and `s2`:

```go
if s1[i] == s2[j]:
    dp[i][j] = 1 + dp[i-1][j-1]
else:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

---

## 🟢 1. Longest Common Subsequence — [LeetCode #1143](https://leetcode.com/problems/longest-common-subsequence)

### ✅ Golang Code:

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
```

### 🧪 Sample I/O:

**Input:** `"abcde"`, `"ace"`  
**Output:** `3`  
**Explanation:** LCS is `"ace"`.

---

## 🟢 2. Longest Palindromic Subsequence — [LeetCode #516](https://leetcode.com/problems/longest-palindromic-subsequence)

### ✅ Golang Code:

```go
func longestPalindromeSubseq(s string) int {
    n := len(s)
    dp := make([][]int, n)
    for i := range dp {
        dp[i] = make([]int, n)
        dp[i][i] = 1
    }

    for length := 2; length <= n; length++ {
        for i := 0; i <= n-length; i++ {
            j := i + length - 1
            if s[i] == s[j] {
                dp[i][j] = 2 + dp[i+1][j-1]
            } else {
                dp[i][j] = max(dp[i+1][j], dp[i][j-1])
            }
        }
    }

    return dp[0][n-1]
}
```

### 🧪 Sample I/O:

**Input:** `"bbbab"`  
**Output:** `4`  
**Explanation:** `"bbbb"` is the longest palindromic subsequence.

---

## 🟢 3. Edit Distance — [LeetCode #72](https://leetcode.com/problems/edit-distance)

### ✅ Golang Code:

```go
func minDistance(word1 string, word2 string) int {
    m, n := len(word1), len(word2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }

    for i := 0; i <= m; i++ {
        dp[i][0] = i
    }
    for j := 0; j <= n; j++ {
        dp[0][j] = j
    }

    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if word1[i-1] == word2[j-1] {
                dp[i][j] = dp[i-1][j-1]
            } else {
                dp[i][j] = 1 + min(dp[i-1][j], min(dp[i][j-1], dp[i-1][j-1]))
            }
        }
    }

    return dp[m][n]
}
```

### 🧪 Sample I/O:

**Input:** `"horse"`, `"ros"`  
**Output:** `3`  
**Explanation:** horse → rorse → rose → ros

---

## 🟢 4. Minimum ASCII Delete Sum — [LeetCode #712](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings)

### ✅ Golang Code:

```go
func minimumDeleteSum(s1 string, s2 string) int {
    m, n := len(s1), len(s2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }

    for i := 1; i <= m; i++ {
        dp[i][0] = dp[i-1][0] + int(s1[i-1])
    }
    for j := 1; j <= n; j++ {
        dp[0][j] = dp[0][j-1] + int(s2[j-1])
    }

    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if s1[i-1] == s2[j-1] {
                dp[i][j] = dp[i-1][j-1]
            } else {
                dp[i][j] = min(dp[i-1][j]+int(s1[i-1]), dp[i][j-1]+int(s2[j-1]))
            }
        }
    }

    return dp[m][n]
}
```

### 🧪 Sample I/O:

**Input:** `"sea"`, `"eat"`  
**Output:** `231`  
**Explanation:** Delete `'s'` (115) and `'t'` (116) → 231

---

## 🟢 5. Shortest Common Supersequence — [LeetCode #1092](https://leetcode.com/problems/shortest-common-supersequence)

### ✅ Golang Code (returns **length**):

```go
func shortestCommonSupersequence(str1 string, str2 string) int {
    m, n := len(str1), len(str2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }

    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if str1[i-1] == str2[j-1] {
                dp[i][j] = 1 + dp[i-1][j-1]
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
            }
        }
    }

    lcs := dp[m][n]
    return m + n - lcs
}
```

### 🧪 Sample I/O:

**Input:** `"abac"`, `"cab"`  
**Output:** `5`  
**Explanation:** One shortest supersequence is `"cabac"`

---

## 🧠 Summary Table

| Problem | Approach | Notes |
|--------|----------|-------|
| LCS | Classic 2D DP | Basis of others |
| Longest Palindromic Subsequence | LCS on `s` and `reverse(s)` | |
| Edit Distance | Replace/Delete/Insert ops | Harder variant |
| Min ASCII Delete | Similar to Edit Distance | Cost-based |
| Shortest Common Supersequence | `m+n - LCS` | |

---
