## **📌 Median of Two Sorted Arrays (LeetCode #4)**

### **🔹 Problem Statement**
Given two sorted arrays `nums1` and `nums2` of size `m` and `n`, find the median of the two sorted arrays.  
The overall run-time complexity should be **O(log(m + n))**.

### **🔹 Example 1**
#### **Input:**
```go
nums1 := []int{1, 3}
nums2 := []int{2}
```
#### **Output:**
```go
2.0
```
#### **Explanation:**
After merging: `[1, 2, 3]`, the median is `2.0`.

---

### **🔹 Example 2**
#### **Input:**
```go
nums1 := []int{1, 2}
nums2 := []int{3, 4}
```
#### **Output:**
```go
2.5
```
#### **Explanation:**
After merging: `[1, 2, 3, 4]`, the median is `(2 + 3) / 2 = 2.5`.

---

## **🔹 Approach**
Since the problem requires **O(log(min(m, n)))** time complexity, we use **binary search on the smaller array** to partition the arrays.

### **🔹 Why Binary Search on the Smaller Array?**
- We divide the arrays into **two halves** such that the **left half** has all smaller elements than the **right half**.
- We perform **binary search** on the smaller array (`nums1`) to find the correct partition.

---

## **🔹 Solution in Golang**
```go
package main

import (
	"fmt"
	"math"
)

func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
	// Ensure nums1 is the smaller array
	if len(nums1) > len(nums2) {
		return findMedianSortedArrays(nums2, nums1)
	}

	x, y := len(nums1), len(nums2)
	low, high := 0, x

	for low <= high {
		partitionX := (low + high) / 2
		partitionY := (x + y + 1) / 2 - partitionX

		// Edge cases: if partitionX is 0, set maxLeftX to -∞
		maxLeftX := math.Inf(-1)
		if partitionX > 0 {
			maxLeftX = float64(nums1[partitionX-1])
		}

		// Edge cases: if partitionX is at the end, set minRightX to +∞
		minRightX := math.Inf(1)
		if partitionX < x {
			minRightX = float64(nums1[partitionX])
		}

		maxLeftY := math.Inf(-1)
		if partitionY > 0 {
			maxLeftY = float64(nums2[partitionY-1])
		}

		minRightY := math.Inf(1)
		if partitionY < y {
			minRightY = float64(nums2[partitionY])
		}

		// Check if we found the correct partition
		if maxLeftX <= minRightY && maxLeftY <= minRightX {
			// If total length is even
			if (x+y)%2 == 0 {
				return (math.Max(maxLeftX, maxLeftY) + math.Min(minRightX, minRightY)) / 2.0
			} else { // If total length is odd
				return math.Max(maxLeftX, maxLeftY)
			}
		} else if maxLeftX > minRightY { // Move towards the left
			high = partitionX - 1
		} else { // Move towards the right
			low = partitionX + 1
		}
	}

	return -1 // Should never reach here
}

func main() {
	nums1 := []int{1, 3}
	nums2 := []int{2}
	fmt.Println(findMedianSortedArrays(nums1, nums2)) // Output: 2.0

	nums1 = []int{1, 2}
	nums2 = []int{3, 4}
	fmt.Println(findMedianSortedArrays(nums1, nums2)) // Output: 2.5
}
```

---

## **🔹 Explanation**
### **Step 1: Ensure `nums1` is the smaller array**
- We always perform **binary search on the smaller array** for efficiency.

### **Step 2: Partition the arrays**
- We divide `nums1` at `partitionX` and `nums2` at `partitionY` such that:
  - **Left half**: `[maxLeftX, maxLeftY]`
  - **Right half**: `[minRightX, minRightY]`

### **Step 3: Check Valid Partition**
- The correct partition satisfies:
  ```
  maxLeftX <= minRightY AND maxLeftY <= minRightX
  ```
- If true, compute the **median**:
  - If **even** length:  
    ```math
    median = (max(maxLeftX, maxLeftY) + min(minRightX, minRightY)) / 2
    ```
  - If **odd** length:
    ```math
    median = max(maxLeftX, maxLeftY)
    ```

### **Step 4: Adjust Partition**
- If `maxLeftX > minRightY`, move **left** (`high = partitionX - 1`).
- If `maxLeftY > minRightX`, move **right** (`low = partitionX + 1`).

---

## **🔹 Complexity Analysis**
| Operation | Complexity |
|-----------|------------|
| **Binary Search on Smaller Array** | **O(log(min(m, n)))** |
| **Finding Median** | **O(1)** |
| **Overall Complexity** | **O(log(min(m, n)))** |

---

## **🔹 Edge Cases Covered**
✔️ **Unequal lengths** → Works with `nums1` and `nums2` of different sizes.  
✔️ **Even/Odd total length** → Computes correctly for both.  
✔️ **One array is empty** → Handles cases like `nums1 = []`, `nums2 = [2,3]`.  
✔️ **All elements in one array are smaller** → Works when `nums1 = [1,2,3]`, `nums2 = [10,11,12]`.  

---

## **🔹 Summary**
✅ **Binary Search on Smaller Array** for **O(log(min(m, n)))** time complexity.  
✅ **Partitioning both arrays** to separate **left and right halves**.  
✅ **Median Calculation** based on array length parity.  

---- 

Let's take the input:  

```go
nums1 := []int{1, 3, 8}
nums2 := []int{7, 9, 10, 11}
```

Total number of elements: **3 + 4 = 7 (odd number of elements)**  
Expected **Median**: `8.0`

---

## **Step-by-Step Execution**
We apply **binary search on the smaller array (`nums1`)**.

#### **Initial Setup**
- `nums1 = [1, 3, 8]`, `nums2 = [7, 9, 10, 11]`
- `x = len(nums1) = 3`, `y = len(nums2) = 4`
- `low = 0`, `high = 3`

### **Iteration 1: Finding the Partition**
- Compute `partitionX = (low + high) / 2 = (0 + 3) / 2 = 1`
- Compute `partitionY = (x + y + 1) / 2 - partitionX = (3 + 4 + 1) / 2 - 1 = 4 - 1 = 3`

Now we check the **left and right halves**:

```
nums1 left half: [1]      | nums1 right half: [3, 8]
nums2 left half: [7, 9, 10] | nums2 right half: [11]
```

- `maxLeftX = nums1[partitionX - 1] = nums1[0] = 1`
- `minRightX = nums1[partitionX] = nums1[1] = 3`
- `maxLeftY = nums2[partitionY - 1] = nums2[2] = 10`
- `minRightY = nums2[partitionY] = nums2[3] = 11`

**Check partition condition:**  
```
maxLeftX (1) <= minRightY (11) ✅
maxLeftY (10) <= minRightX (3) ❌ (Condition Fails)
```

Since **maxLeftY > minRightX**, **move right** (`low = partitionX + 1 = 2`).

---

### **Iteration 2: Adjusting Partition**
- Compute `partitionX = (2 + 3) / 2 = 2`
- Compute `partitionY = 4 - 2 = 2`

Now:
```
nums1 left half: [1, 3]      | nums1 right half: [8]
nums2 left half: [7, 9] | nums2 right half: [10, 11]
```

- `maxLeftX = nums1[1] = 3`
- `minRightX = nums1[2] = 8`
- `maxLeftY = nums2[1] = 9`
- `minRightY = nums2[2] = 10`

**Check partition condition:**  
```
maxLeftX (3) <= minRightY (10) ✅
maxLeftY (9) <= minRightX (8) ❌ (Condition Fails)
```

Since **maxLeftY > minRightX**, **move right** (`low = partitionX + 1 = 3`).

---

### **Iteration 3: Final Partition**
- Compute `partitionX = (3 + 3) / 2 = 3`
- Compute `partitionY = 4 - 3 = 1`

Now:
```
nums1 left half: [1, 3, 8] | nums1 right half: []
nums2 left half: [7]       | nums2 right half: [9, 10, 11]
```

- `maxLeftX = nums1[2] = 8`
- `minRightX = +∞` (no right elements left)
- `maxLeftY = nums2[0] = 7`
- `minRightY = nums2[1] = 9`

**Check partition condition:**  
```
maxLeftX (8) <= minRightY (9) ✅
maxLeftY (7) <= minRightX (∞) ✅
```
✅ **Valid Partition Found!**

Since `(3+4)` is **odd**, the median is:
```go
median = max(maxLeftX, maxLeftY) = max(8, 7) = 8.0
```

---

### **Final Output**
```go
Output: 8.0
```

---

## **💡 Summary of Iterations**
| Iteration | `partitionX` | `partitionY` | `maxLeftX` | `minRightX` | `maxLeftY` | `minRightY` | Partition Valid? | Action |
|-----------|-------------|-------------|------------|------------|------------|------------|-----------------|---------|
| 1 | 1 | 3 | 1 | 3 | 10 | 11 | ❌ (maxLeftY > minRightX) | Move Right |
| 2 | 2 | 2 | 3 | 8 | 9 | 10 | ❌ (maxLeftY > minRightX) | Move Right |
| 3 | 3 | 1 | 8 | ∞ | 7 | 9 | ✅ (Condition Met) | Return Median |

---

## **🔹 Key Takeaways**
1. **Binary search is applied on the smaller array** to improve efficiency.
2. **Partitioning divides the arrays** into left and right halves.
3. **Valid partition satisfies** `maxLeftX <= minRightY` and `maxLeftY <= minRightX`.
4. **For even length**, median is `(max(maxLeftX, maxLeftY) + min(minRightX, minRightY)) / 2.0`.
5. **For odd length**, median is `max(maxLeftX, maxLeftY)`.
