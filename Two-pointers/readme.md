### Two pointers


#### Problem 1 : Valid Palindrome


```
func isPalindrome(s string) bool {
    s = strings.ToLower(s) // convert to lowercase
    left := 0
    right := len(s) - 1

    for left < right {
        // skip non-alphanumeric characters
        for left < right && !isAlphaNumeric(s[left]) {
            left++
        }
        for left < right && !isAlphaNumeric(s[right]) {
            right--
        }

        // compare characters
        if s[left] != s[right] {
            return false
        }

        left++
        right--
    }

    return true
}

func isAlphaNumeric(char byte) bool {
    return (char >= 'a' && char <= 'z') ||
           (char >= 'A' && char <= 'Z') ||
           (char >= '0' && char <= '9')
}


```





Here, the function isPalindrome takes a string s and returns a boolean value indicating whether or not the string is a valid palindrome.

The two pointers left and right start at the beginning and end of the string, respectively. The function then skips over any non-alphanumeric characters (using the isAlphaNumeric function) and compares the characters at left and right. If they don't match, the function returns false. If they do match, the pointers move inward and the process continues until the pointers meet in the middle, at which point the function returns true.

Note that this implementation assumes that the input string only contains ASCII characters. If the input string contains Unicode characters, the implementation may need to be modified to handle them correctly.


#### Problem 2 : 3 Sum

```
func threeSum(nums []int) [][]int {
    sort.Ints(nums) // sort the input array in non-decreasing order
    var result [][]int

    for i := 0; i < len(nums)-2; i++ {
        // skip duplicates
        if i > 0 && nums[i] == nums[i-1] {
            continue
        }

        left := i+1
        right := len(nums)-1

        for left < right {
            sum := nums[i] + nums[left] + nums[right]

            if sum == 0 {
                result = append(result, []int{nums[i], nums[left], nums[right]})

                // skip duplicates
                for left < right && nums[left] == nums[left+1] {
                    left++
                }
                for left < right && nums[right] == nums[right-1] {
                    right--
                }

                left++
                right--
            } else if sum < 0 {
                left++
            } else {
                right--
            }
        }
    }

    return result
}



```


Here, the function threeSum takes an integer array nums and returns a two-dimensional array containing all unique triplets that sum to zero.

The function first sorts the input array in non-decreasing order. It then loops through the array, skipping duplicates. For each element, it uses two pointers (left and right) to search for two other elements in the array that sum to the negative of the current element. If such elements are found, the function appends the triplet to the result and skips over any duplicates.


#### Problem 3 : Container with most water

```
func maxArea(height []int) int {
    maxArea := 0
    left := 0
    right := len(height) - 1

    for left < right {
        h := min(height[left], height[right])
        maxArea = max(maxArea, h*(right-left))

        if height[left] < height[right] {
            left++
        } else {
            right--
        }
    }

    return maxArea
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


Here, the function maxArea takes an integer array height representing the heights of lines on a container, and returns the maximum amount of water that can be contained within the container.

The function initializes the maximum area maxArea to 0 and sets two pointers, left and right, to the beginning and end of the array, respectively. It then loops through the array, calculating the area of the container represented by the two lines at the current left and right indices, and updating maxArea if the calculated area is greater. The height of the container is the minimum of the two line heights, and the width is the difference between the left and right indices.

The function then updates the left and right pointers by moving the pointer that points to the shorter line, in order to find the next pair of lines that might form a container with greater area.

The functions min and max are helper functions to return the minimum and maximum of two integers, respectively.


### Problem 4: Trapping rain water

Sure! Below is the **Golang implementation** for the **Trapping Rain Water** problem.  

---

## **🔹 Problem Statement**
Given an array **`height[]`** representing elevation heights, calculate **how much rainwater can be trapped** after raining.

### **Example**
```go
Input:  height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```
💧 **Visual Representation**  
```
        #
  #     ##   #
  #  #  ## # ##
 --------------
 0  1  0  2  1 ...
```
🔹 **Water is trapped** between bars at different elevations.

---

# **🔹 Solution Approaches**
### **1️⃣ Two-Pointer Approach (O(n) Time, O(1) Space)**
- Use **two pointers** (`left` and `right`) to scan from both sides.
- Maintain **`left_max`** and **`right_max`**.
- If `left_max < right_max`, move **left pointer** and trap water.
- Otherwise, move **right pointer**.

---

## **✅ Golang Implementation (Two-Pointer Approach)**
```go
package main

import "fmt"

// Trap calculates the total trapped water using two-pointer approach
func Trap(height []int) int {
    if len(height) == 0 {
        return 0
    }

    left, right := 0, len(height)-1
    leftMax, rightMax := 0, 0
    trappedWater := 0

    for left < right {
        if height[left] < height[right] {
            if height[left] >= leftMax {
                leftMax = height[left] // Update left max
            } else {
                trappedWater += leftMax - height[left] // Water trapped at current left
            }
            left++ // Move left pointer
        } else {
            if height[right] >= rightMax {
                rightMax = height[right] // Update right max
            } else {
                trappedWater += rightMax - height[right] // Water trapped at current right
            }
            right-- // Move right pointer
        }
    }

    return trappedWater
}

func main() {
    height := []int{0,1,0,2,1,0,1,3,2,1,2,1}
    fmt.Println("Trapped Water:", Trap(height)) // Output: 6
}
```

---

## **🔹 How It Works**
| Step | Left | Right | Left Max | Right Max | Water Trapped |
|------|------|-------|----------|-----------|---------------|
| 1    | 0    | 11    | 0        | 1         | 0             |
| 2    | 1    | 11    | 1        | 1         | 0             |
| 3    | 2    | 11    | 1        | 1         | 1             |
| 4    | 3    | 11    | 2        | 1         | 1             |
| 5    | 3    | 10    | 2        | 2         | 1             |
| 6    | 4    | 10    | 2        | 2         | 2             |
| 7    | 5    | 10    | 2        | 2         | 3             |
| 8    | 6    | 10    | 2        | 2         | 3             |
| 9    | 7    | 10    | 3        | 2         | 3             |

🔹 **Final Trapped Water** = `6`

---

## **🔹 Time & Space Complexity**
| Approach | Time Complexity | Space Complexity |
|----------|----------------|-----------------|
| **Two-Pointer** | `O(n)` | `O(1)` |

---

## **🔥 Alternative Approach: Dynamic Programming (O(n) Time, O(n) Space)**
- Precompute **leftMax[]** and **rightMax[]** for each index.
- Water trapped at `i` is `min(leftMax[i], rightMax[i]) - height[i]`.

```go
package main

import "fmt"

func TrapDP(height []int) int {
    if len(height) == 0 {
        return 0
    }

    n := len(height)
    leftMax := make([]int, n)
    rightMax := make([]int, n)
    trappedWater := 0

    // Compute left max for each index
    leftMax[0] = height[0]
    for i := 1; i < n; i++ {
        leftMax[i] = max(leftMax[i-1], height[i])
    }

    // Compute right max for each index
    rightMax[n-1] = height[n-1]
    for i := n - 2; i >= 0; i-- {
        rightMax[i] = max(rightMax[i+1], height[i])
    }

    // Calculate trapped water
    for i := 0; i < n; i++ {
        trappedWater += min(leftMax[i], rightMax[i]) - height[i]
    }

    return trappedWater
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func main() {
    height := []int{0,1,0,2,1,0,1,3,2,1,2,1}
    fmt.Println("Trapped Water (DP):", TrapDP(height)) // Output: 6
}
```

---

## **🔹 Comparison of Approaches**
| Approach | Time Complexity | Space Complexity | Best When? |
|----------|----------------|-----------------|-------------|
| **Two-Pointer** | `O(n)` | `O(1)` | Best for efficiency |
| **Dynamic Programming** | `O(n)` | `O(n)` | When space is not a constraint |

---

## **🚀 Key Takeaways**
✅ **Two-pointer approach** is optimal (`O(n) time, O(1) space`).  
✅ **Dynamic programming** is useful for **understanding how trapped water accumulates**.  
✅ **Always use `O(1)` space when possible** for competitive programming & interviews.








