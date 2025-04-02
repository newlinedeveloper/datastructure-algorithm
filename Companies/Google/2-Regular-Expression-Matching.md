## **Regular Expression Matching in Golang**
This problem is about matching a given string `s` against a pattern `p` that may contain:
- **`.`** → Matches any single character.
- **`*`** → Matches zero or more of the preceding character.

We need to implement `isMatch(s, p) bool`, which returns `true` if `s` fully matches `p`.

---

## **🔹 Dynamic Programming Approach**
We solve this using **bottom-up dynamic programming (DP)**.  
Let `dp[i][j]` represent whether `s[0:i]` matches `p[0:j]`.

### **Recurrence Relation:**
1. **If characters match (or pattern has `.`):**
   - `dp[i][j] = dp[i-1][j-1]`
  
2. **If `p[j-1]` is `*`:**
   - Ignore `p[j-2]` and `*`: `dp[i][j] = dp[i][j-2]` (zero occurrences)
   - Use `p[j-2]` if it matches `s[i-1]`: `dp[i][j] = dp[i-1][j]` (one or more occurrences)

---

## **🔹 Golang Solution**
```go
package main

import "fmt"

func isMatch(s string, p string) bool {
    m, n := len(s), len(p)
    dp := make([][]bool, m+1)
    for i := range dp {
        dp[i] = make([]bool, n+1)
    }
    
    // Empty string matches empty pattern
    dp[0][0] = true

    // Handle patterns like "a*", "a*b*", ".*"
    for j := 1; j <= n; j++ {
        if p[j-1] == '*' {
            dp[0][j] = dp[0][j-2]
        }
    }

    // Fill DP table
    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if p[j-1] == s[i-1] || p[j-1] == '.' {
                dp[i][j] = dp[i-1][j-1]
            } else if p[j-1] == '*' {
                dp[i][j] = dp[i][j-2] || (dp[i-1][j] && (s[i-1] == p[j-2] || p[j-2] == '.'))
            }
        }
    }

    return dp[m][n]
}

func main() {
    fmt.Println(isMatch("aa", "a"))      // false
    fmt.Println(isMatch("aa", "a*"))     // true
    fmt.Println(isMatch("ab", ".*"))     // true
    fmt.Println(isMatch("aab", "c*a*b")) // true
    fmt.Println(isMatch("mississippi", "mis*is*p*.")) // false
}
```

---

## **🔹 Explanation with Example**
### **Example 1**
```go
s = "aa"
p = "a*"
```
**DP Table Construction:**
| s \ p | "" | a | * |
|----|----|----|----|
| "" | T | F | **T** |
| a  | F | **T** | T |
| a  | F | F | **T** |

🔹 Final output: `true` (since `"a*"` can match `"aa"`)

---

## **🔹 Complexity Analysis**
- **Time Complexity:** `O(m * n)`, where `m` is the length of `s` and `n` is the length of `p`.
- **Space Complexity:** `O(m * n)` for the DP table.

---

## **💡 Summary**
✅ **DP Table tracks whether prefixes match**  
✅ **`*` can remove or extend characters**  
✅ **Uses bottom-up DP approach**
Let's break down how the DP table is filled step by step for the input:  
```go
s = "mississippi"
p = "mis*is*p*."
```

---

### **🔹 Understanding the Pattern `"mis*is*p*."`**
- `mis*` → Matches `"mis"` (the `s*` means zero or more `s`, so it can match `"mis"`)  
- `is*` → Matches `"is"` (again, `s*` can match `"s"`)  
- `p*` → Matches `""` (zero occurrences of `p`)  
- `.` → Matches any single character (`i` in `"mississippi"`)  

---

## **Step-by-Step DP Table Construction**
We construct a **(m+1) x (n+1) DP table** where `m = len(s) = 11`, `n = len(p) = 10`.

| `s \ p` | ""  | m  | i  | s  | *  | i  | s  | *  | p  | *  | .  |
|------|----|----|----|----|----|----|----|----|----|----|----|
| ""   | T  | F  | F  | F  | **T**  | F  | F  | **T**  | F  | **T**  | F  |
| m    | F  | **T**  | F  | F  | T  | F  | F  | T  | F  | T  | F  |
| i    | F  | F  | **T**  | F  | T  | **T**  | F  | T  | F  | T  | F  |
| s    | F  | F  | F  | **T**  | **T**  | F  | T  | T  | F  | T  | F  |
| s    | F  | F  | F  | **T**  | **T**  | F  | **T**  | T  | F  | T  | F  |
| i    | F  | F  | **T**  | F  | T  | **T**  | F  | T  | F  | T  | F  |
| s    | F  | F  | F  | **T**  | **T**  | F  | **T**  | T  | F  | T  | F  |
| s    | F  | F  | F  | **T**  | **T**  | F  | **T**  | T  | F  | T  | F  |
| i    | F  | F  | **T**  | F  | T  | **T**  | F  | T  | F  | T  | F  |
| p    | F  | F  | F  | F  | T  | F  | F  | T  | **T**  | **T**  | F  |
| p    | F  | F  | F  | F  | T  | F  | F  | T  | **T**  | **T**  | F  |
| i    | F  | F  | **T**  | F  | T  | **T**  | F  | T  | F  | T  | **T**  |

✅ **Final result:** `dp[11][10] = False`

---

## **🔹 Why is the answer `false`?**
- `mis*is*p*.` should match `"mississippi"`, but the last `.` needs exactly **one character** to match.
- The pattern `p*` allows zero `p`s, but it does not match the last `"i"`.
- If the pattern ended with `.*`, it would match, but since it ends with `.`, it fails.

---

## **🔹 Fixing the Pattern**
✅ If the pattern was `"mis*is*p*i"`, it would return `true`.  
✅ If the pattern was `"mis*is*p*."`, but the string was `"mississipp"`, it would also return `true`.

------

### **Regular Expression Matching – Top-Down Approach (Memoization)**
We will solve the problem using **Top-Down Dynamic Programming (Memoization)**. Instead of building the DP table bottom-up, we use recursion with caching to avoid redundant calculations.

---

## **🔹 Approach**
1. **Recursive Definition**:
   - If `p[j]` is **not** a `*`, check if `s[i]` matches `p[j]` and recurse on `i+1, j+1`.
   - If `p[j]` is a `*`, we have **two choices**:
     - Ignore `p[j-1]` and `p[j]` (treat `p[j-1]*` as empty).
     - Use `p[j-1]` (only if `s[i]` matches `p[j-1]`).
   
2. **Base Cases**:
   - If `p` is empty, return `true` if `s` is also empty, otherwise `false`.
   - If `s` is empty but `p` contains only `x*` patterns, return `true`.

3. **Memoization**:
   - Store already computed `(i, j)` results in a cache to avoid redundant calculations.

---

### **🔹 Implementation in Golang**
```go
package main

import (
	"fmt"
)

// Memoization map
var memo map[string]bool

func isMatch(s string, p string) bool {
	memo = make(map[string]bool) // Initialize cache
	return dp(0, 0, s, p)
}

// Recursive DP function with memoization
func dp(i int, j int, s string, p string) bool {
	// Check memoization cache
	key := fmt.Sprintf("%d,%d", i, j)
	if val, found := memo[key]; found {
		return val
	}

	// Base case: If both `s` and `p` are fully matched
	if j == len(p) {
		return i == len(s)
	}

	// First match condition: If s[i] and p[j] match or p[j] is '.'
	firstMatch := i < len(s) && (s[i] == p[j] || p[j] == '.')

	// If the next character in `p` is `*`, handle two cases:
	if j+1 < len(p) && p[j+1] == '*' {
		memo[key] = dp(i, j+2, s, p) || (firstMatch && dp(i+1, j, s, p))
	} else {
		memo[key] = firstMatch && dp(i+1, j+1, s, p)
	}

	return memo[key]
}

func main() {
	fmt.Println(isMatch("mississippi", "mis*is*p*.")) // false
	fmt.Println(isMatch("aa", "a*"))                 // true
	fmt.Println(isMatch("ab", ".*"))                 // true
	fmt.Println(isMatch("aab", "c*a*b"))             // true
}
```

---

## **🔹 How It Works (Example Walkthrough)**
**Input:**  
`s = "mississippi"`  
`p = "mis*is*p*."`

### **1st Call** → `dp(0,0, s, p)`
- `s[0] = 'm'`, `p[0] = 'm'` → **Match**
- Recurse on `dp(1,1, s, p)`

### **2nd Call** → `dp(1,1, s, p)`
- `s[1] = 'i'`, `p[1] = 'i'` → **Match**
- Recurse on `dp(2,2, s, p)`

### **3rd Call** → `dp(2,2, s, p)`
- `s[2] = 's'`, `p[2] = 's'` → **Match**
- `p[3] = '*'`, so we have:
  1. Ignore `"s*"` (move to `dp(2,4, s, p)`)
  2. Use `"s*"` (move to `dp(3,2, s, p)`, consuming an `"s"`)

### **Handling '*' Cases**
- The recursion will try both options and memoize results to avoid redundant work.

---

## **🔹 Time & Space Complexity**
- **Time Complexity:** `O(m * n)` (since each `(i, j)` is computed once).
- **Space Complexity:** `O(m * n)` (memoization table).

