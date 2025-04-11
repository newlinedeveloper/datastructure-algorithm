Let's dive into the **String Segmentation / Word Break Pattern** — commonly used when you need to check or build valid substrings using dictionary words or satisfy a given logic like palindromes.

---

## 🧩 5. String Segmentation / Word Break Pattern

---

### 🔍 **How to Identify This Pattern**

Look for problems where:

| Clue in Problem Statement | Hints at |
|---------------------------|----------|
| "Split the string into valid words from a dictionary" | Word Break |
| "Return all possible valid sentences" | Word Break II |
| "Partition such that each part is a palindrome" | Palindrome Partitioning |
| "Check if string can be segmented..." | DP over string prefixes |

**Idea**: At each position `i` in the string, try all substrings `s[j:i]` and check if they’re valid (e.g., in dictionary or a palindrome).

---

## 🟢 1. Word Break — [LeetCode #139](https://leetcode.com/problems/word-break)

### ✅ Problem
> Given a string `s` and a dictionary of strings `wordDict`, return true if `s` can be segmented into a space-separated sequence of one or more dictionary words.

---

### ✅ Golang Code:

```go
func wordBreak(s string, wordDict []string) bool {
    wordSet := make(map[string]bool)
    for _, word := range wordDict {
        wordSet[word] = true
    }

    dp := make([]bool, len(s)+1)
    dp[0] = true // base case: empty string

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

### 🧪 Sample I/O:

**Input:** `s = "leetcode", wordDict = ["leet", "code"]`  
**Output:** `true`

---

## 🟢 2. Palindrome Partitioning — [LeetCode #131](https://leetcode.com/problems/palindrome-partitioning)

### ✅ Problem
> Given a string `s`, partition `s` such that every substring is a palindrome. Return all such partitions.

---

### ✅ Golang Code:

```go
func partition(s string) [][]string {
    var res [][]string
    var path []string

    var isPalindrome = func(l, r int) bool {
        for l < r {
            if s[l] != s[r] {
                return false
            }
            l++
            r--
        }
        return true
    }

    var dfs func(start int)
    dfs = func(start int) {
        if start == len(s) {
            tmp := make([]string, len(path))
            copy(tmp, path)
            res = append(res, tmp)
            return
        }

        for end := start; end < len(s); end++ {
            if isPalindrome(start, end) {
                path = append(path, s[start:end+1])
                dfs(end + 1)
                path = path[:len(path)-1]
            }
        }
    }

    dfs(0)
    return res
}
```

### 🧪 Sample I/O:

**Input:** `"aab"`  
**Output:** `[["a","a","b"],["aa","b"]]`

---

## 🟢 3. Word Break II — [LeetCode #140](https://leetcode.com/problems/word-break-ii)

### ✅ Problem
> Return all possible sentences where words are from the dictionary and form the input string.

---

### ✅ Golang Code:

```go
func wordBreak(s string, wordDict []string) []string {
    wordSet := make(map[string]bool)
    for _, word := range wordDict {
        wordSet[word] = true
    }

    memo := make(map[string][]string)

    var dfs func(string) []string
    dfs = func(s string) []string {
        if val, ok := memo[s]; ok {
            return val
        }

        var res []string
        if wordSet[s] {
            res = append(res, s)
        }

        for i := 1; i < len(s); i++ {
            suffix := s[i:]
            if wordSet[suffix] {
                prefix := s[:i]
                for _, pre := range dfs(prefix) {
                    res = append(res, pre+" "+suffix)
                }
            }
        }

        memo[s] = res
        return res
    }

    return dfs(s)
}
```

### 🧪 Sample I/O:

**Input:** `s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]`  
**Output:** `["cats and dog","cat sand dog"]`

---

## 🧠 Summary Table

| Problem | Goal | Technique |
|--------|------|-----------|
| Word Break | Can `s` be split into dict words | 1D DP with prefix checks |
| Palindrome Partitioning | All splits with palindromes | Backtracking + isPalindrome |
| Word Break II | All valid sentence splits | DFS + memoization |

---
