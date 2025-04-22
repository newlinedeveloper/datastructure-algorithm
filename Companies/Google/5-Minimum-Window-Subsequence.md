The **Minimum Window Subsequence** problem is a variation of the Minimum Window Substring problem. Here's the formal problem:

---

### 💬 Problem Statement:

Given two strings `S` and `T`, return the **minimum window** in `S` which will contain **all the characters** in `T** in order** (i.e., as a **subsequence**). If there is no such window in `S` that covers all characters in `T`, return the empty string `""`.

---

### 🔍 Example:

```txt
Input:  S = "abcdebdde", T = "bde"
Output: "bcde"
Explanation: "bcde" is the smallest window in S where T = "bde" is a subsequence.
```

---

### 🧠 Intuition:

- It's not about finding a permutation or anagrams (like in Minimum Window Substring).
- We need to find a substring of `S` such that `T` appears **in order** (not necessarily contiguous).
- The idea is to **scan** `S` and try to find a subsequence that matches `T`.
- Once a match is found, we **go back** to find the start of the window.

---

### ✅ Go Solution:

```go
func minWindow(S string, T string) string {
    m, n := len(S), len(T)
    minLen := m + 1
    startIdx := -1

    for i := 0; i < m; i++ {
        if S[i] != T[0] {
            continue
        }

        sIdx := i
        tIdx := 0

        // Move forward to check if T is a subsequence starting from i
        for sIdx < m && tIdx < n {
            if S[sIdx] == T[tIdx] {
                tIdx++
            }
            sIdx++
        }

        if tIdx == n {
            // Now backtrack to minimize the window
            end := sIdx
            tIdx = n - 1
            sIdx--

            for tIdx >= 0 {
                if S[sIdx] == T[tIdx] {
                    tIdx--
                }
                sIdx--
            }

            sIdx++
            if end - sIdx < minLen {
                minLen = end - sIdx
                startIdx = sIdx
            }
        }
    }

    if startIdx == -1 {
        return ""
    }

    return S[startIdx : startIdx + minLen]
}
```

---

### 🧪 Example Dry Run:

```go
S = "abcdebdde"
T = "bde"
```

1. Start at `S[1] = 'b'`, matches `T[0]`
2. Move forward, match `'d'` → then `'e'`
3. Found valid window `S[1:5] = "bcde"`
4. Backtrack to minimize → `sIdx = 1`, final window is "bcde"
5. Continue scanning, but no shorter valid subsequence found.

---

### ⏱ Time Complexity:

- **Worst Case O(M * N)**  
  - `M` = length of `S`, `N` = length of `T`
  - Because we may scan `S` multiple times and check full `T` each time.

---
