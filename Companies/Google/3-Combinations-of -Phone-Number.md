### **📌 Letter Combinations of a Phone Number (Leetcode 17)**
Given a string containing digits from `2-9`, return all possible letter combinations that the number could represent.

📌 **Mapping of digits to letters (like on a telephone keypad):**
```
2 -> "abc"
3 -> "def"
4 -> "ghi"
5 -> "jkl"
6 -> "mno"
7 -> "pqrs"
8 -> "tuv"
9 -> "wxyz"
```

---

## **🔹 Approach: Backtracking**
1. **Use a Map** to store digit-to-letters mapping.
2. **Backtracking**:
   - Start from the first digit and explore all possible letter choices.
   - Move to the next digit and append new letters.
   - When the path length equals the input string length, add it to results.
3. **Base Case**:
   - If input is empty, return an empty list.
   - If we reach the end of the string, add the combination to results.

---

## **🔹 Golang Solution**
```go
package main

import "fmt"

var digitMap = map[byte]string{
	'2': "abc",
	'3': "def",
	'4': "ghi",
	'5': "jkl",
	'6': "mno",
	'7': "pqrs",
	'8': "tuv",
	'9': "wxyz",
}

func letterCombinations(digits string) []string {
	if len(digits) == 0 {
		return []string{}
	}

	var result []string
	backtrack(digits, 0, "", &result)
	return result
}

// Backtracking function
func backtrack(digits string, index int, path string, result *[]string) {
	if index == len(digits) {
		*result = append(*result, path)
		return
	}

	letters := digitMap[digits[index]]
	for i := 0; i < len(letters); i++ {
		backtrack(digits, index+1, path+string(letters[i]), result)
	}
}

func main() {
	fmt.Println(letterCombinations("23")) // ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
	fmt.Println(letterCombinations(""))   // []
	fmt.Println(letterCombinations("2"))  // ["a", "b", "c"]
}
```

---

## **🔹 How It Works (Example Walkthrough)**
### **Input:** `"23"`
```
Mapping: 
'2' → ["a", "b", "c"]
'3' → ["d", "e", "f"]
```

### **Backtracking Steps:**
1. Start with an empty string `""`.
2. Add each letter from `"2" → "a", "b", "c"`, then move to `"3"`.
3. For each letter in `"2"`, append `"3"`’s letters (`"d", "e", "f"`).
4. Store all combinations.

📌 **Recursive Calls:**  
```
("", 0) → ("a", 1) → ("ad", 2) ✅  
                 → ("ae", 2) ✅  
                 → ("af", 2) ✅  
        → ("b", 1) → ("bd", 2) ✅  
                 → ("be", 2) ✅  
                 → ("bf", 2) ✅  
        → ("c", 1) → ("cd", 2) ✅  
                 → ("ce", 2) ✅  
                 → ("cf", 2) ✅  
```

---

## **🔹 Time & Space Complexity**
- **Time Complexity:** `O(4^N)`, where `N` is the number of digits.
  - Each digit has at most **4** letter choices (`7 & 9` have 4 letters).
- **Space Complexity:** `O(N)`, since recursion goes at most `N` deep.

This approach efficiently generates all possible combinations using **backtracking**. 🚀
