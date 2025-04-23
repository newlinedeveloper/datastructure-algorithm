This problem is asking for the **maximum length** of a **substring** where:

- The **first character is lexicographically smaller** than the **last character**.
- You need to solve this in **O(n)** time for up to `10^5` characters.
- Characters are lowercase English letters (`a` to `z`).

---

### 🔍 Problem Breakdown

We're not finding any specific substring — just the **maximum length** of a valid substring, where:

- `s[i] < s[j]` and
- the substring starts at `i`, ends at `j`, and `j > i`.

---

### ✅ Observations

1. You **don’t need to check every possible substring** (which would take O(n²) time).
2. You only care about:
   - The **first occurrence** of every character.
   - The **last occurrence** of every character.
3. To solve in linear time, we can:
   - Iterate once to collect first/last occurrences.
   - Iterate over all possible character pairs (only 26 x 26 = 676 pairs).

---

### ✅ Golang Code

```go
package main

import (
	"fmt"
)

func maxSubstringLength(s string) int {
	first := make([]int, 26)
	last := make([]int, 26)

	for i := 0; i < 26; i++ {
		first[i] = -1
		last[i] = -1
	}

	// Record first and last occurrences
	for i, ch := range s {
		idx := ch - 'a'
		if first[idx] == -1 {
			first[idx] = i
		}
		last[idx] = i
	}

	maxLen := 0

	// Try all pairs (c1, c2) where c1 < c2
	for i := 0; i < 26; i++ {
		for j := 0; j < 26; j++ {
			if i < j && first[i] != -1 && last[j] != -1 {
				if last[j] > first[i] {
					maxLen = max(maxLen, last[j]-first[i]+1)
				}
			}
		}
	}

	return maxLen
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	s := "dbabcb"
	fmt.Println(maxSubstringLength(s)) // Output: 4
}
```

---

### 🧠 Explanation

Given `"dbabcb"`:

- First occurrences: `d → 0`, `b → 1`, `a → 2`, `c → 4`
- Last occurrences: `d → 0`, `b → 5`, `a → 2`, `c → 4`

Now check all `(first_char, last_char)` where `first_char < last_char` lexicographically. One valid pair:
- `a (index 2)` and `c (index 4)` → substring `"abc"` → length `4 - 2 + 1 = 3`
- `b (index 1)` and `c (index 4)` → `"babcb"` → 4

Maximum is `4`.

---

### ✅ Time and Space Complexity

- **Time Complexity**: `O(n + 26^2)` → linear for practical input sizes.
- **Space Complexity**: `O(26)` for first/last occurrence maps.
