This is a **string template resolution** problem, where templates can be **nested**. The input contains placeholders like `%X%`, and the values in the map can also reference other templates.

---

### ✅ Problem Breakdown:
Given:
```go
templateMap := map[string]string{
    "X": "123",
    "Y": "456_%X%",
}
input := "%X%_%Y%"
```

Expected Output:  
`123_456_123`

Notice:
- `%X%` resolves to `"123"`
- `%Y%` resolves to `"456_%X%"` → which again contains `%X%`, so it becomes `"456_123"`
- So full input becomes: `123_456_123`

---

### 🧠 Key Idea:
1. Recursively resolve all variables in the map.
2. Then apply them to the input string.

---

### ✅ Go Solution:

```go
package main

import (
	"fmt"
	"regexp"
	"strings"
)

func resolveTemplates(templateMap map[string]string, input string) string {
	resolved := make(map[string]string)

	// Recursively resolve a key
	var resolve func(key string) string
	resolve = func(key string) string {
		if val, ok := resolved[key]; ok {
			return val
		}
		val := templateMap[key]
		val = replacePlaceholders(val, resolve)
		resolved[key] = val
		return val
	}

	// Replace placeholders like %X% using the resolve function
	result := replacePlaceholders(input, resolve)
	return result
}

// Helper to replace all %KEY% using a resolver function
func replacePlaceholders(s string, resolver func(string) string) string {
	re := regexp.MustCompile(`%([A-Za-z0-9_]+)%`)
	return re.ReplaceAllStringFunc(s, func(m string) string {
		key := strings.Trim(m, "%")
		return resolver(key)
	})
}

func main() {
	templateMap := map[string]string{
		"X": "123",
		"Y": "456_%X%",
	}
	input := "%X%_%Y%"

	output := resolveTemplates(templateMap, input)
	fmt.Println(output) // Output: 123_456_123
}
```

---

### ✅ Output:
```
123_456_123
```

---

### 💡 Pattern:
This is a **recursive template resolver** / **variable substitution** problem.

- Use **memoization** to avoid recomputing keys.
- Use **regular expressions** to find and replace `%KEY%` patterns.
- Detect circular references (optional but good in real-world use cases).

---

Let me know if you want a version with **cycle detection** or a **non-recursive** approach.
