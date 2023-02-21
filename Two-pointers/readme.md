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






