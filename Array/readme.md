### Array

#### Contains Duplicate

```
package main

import "fmt"

func containsDuplicate(nums []int) bool {
    seen := make(map[int]bool)

    for _, num := range nums {
        if seen[num] {
            return true // Duplicate found
        }
        seen[num] = true
    }

    return false // No duplicates found
}

func main() {
    nums := []int{1, 2, 3, 4, 5}
    fmt.Println("Contains duplicate:", containsDuplicate(nums))

    nums = []int{1, 2, 3, 1, 4, 5}
    fmt.Println("Contains duplicate:", containsDuplicate(nums))
}


```
In the containsDuplicate function, we create a hashmap seen to keep track of the numbers we have encountered so far. We iterate through the nums array, and for each number, we check if it already exists in the seen map. If it does, we return true indicating that a duplicate is found. Otherwise, we mark the number as seen by setting its value in the map to true. Finally, if we reach the end of the loop without finding any duplicates, we return false.

In the main function, we demonstrate the usage of the containsDuplicate function with two example arrays. The first array does not contain any duplicates, while the second array contains a duplicate number. The output will indicate whether each array contains a duplicate or not.

#### Valid Anagram

```
package main

import "fmt"

func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false // Different lengths, not anagrams
    }

    count := make(map[rune]int)

    // Count the occurrences of each character in string s
    for _, char := range s {
        count[char]++
    }

    // Subtract the occurrences of each character in string t
    // If the count goes below zero, it means the characters in t are not anagrams of s
    for _, char := range t {
        count[char]--
        if count[char] < 0 {
            return false
        }
    }

    return true // All characters match, valid anagrams
}

func main() {
    s1 := "anagram"
    t1 := "nagaram"
    fmt.Println("Is anagram:", isAnagram(s1, t1))

    s2 := "rat"
    t2 := "car"
    fmt.Println("Is anagram:", isAnagram(s2, t2))
}


```

In the isAnagram function, we first check if the lengths of the two input strings s and t are equal. If they have different lengths, they cannot be anagrams, so we return false.

We create a hashmap count to store the count of each character in s. We iterate over s, and for each character, we increment its count in the count map.

Next, we iterate over t, and for each character, we decrement its count in the count map. If at any point the count goes below zero, it means the characters in t are not anagrams of s, so we return false.

If we reach the end of the loop without finding any discrepancies, it means all characters match and the strings are valid anagrams, so we return true.

In the main function, we demonstrate the usage of the isAnagram function with two example strings. The first pair of strings are valid anagrams, while the second pair are not. The output will indicate whether each pair of strings is an anagram or not.



#### Two Sum

```
package main

import "fmt"

func twoSum(nums []int, target int) []int {
    numMap := make(map[int]int)

    for i, num := range nums {
        complement := target - num
        if index, found := numMap[complement]; found {
            return []int{index, i}
        }
        numMap[num] = i
    }

    return nil // No two numbers add up to the target
}

func main() {
    nums := []int{2, 7, 11, 15}
    target := 9

    result := twoSum(nums, target)
    if result != nil {
        fmt.Println("Indices:", result)
        fmt.Println("Numbers:", nums[result[0]], nums[result[1]])
    } else {
        fmt.Println("No two numbers add up to the target")
    }
}


```

In the twoSum function, we iterate over the nums slice and for each number, we calculate the complement by subtracting it from the target. We then check if the complement exists as a key in the numMap. If it does, it means we have found the pair of numbers that add up to the target, so we return their indices as a slice.

If the complement is not found in the numMap, we store the current number and its index in the map for future reference.

In the main function, we provide an example nums slice and a target value. We call the twoSum function and check the result. If a valid pair of numbers is found, we print their indices and corresponding values. If no two numbers add up to the target, we print a message indicating that.

Note: This implementation assumes that there is exactly one solution, and each input has exactly one valid answer



#### Group Anagrams

```
package main

import (
	"fmt"
	"sort"
	"strings"
)

func groupAnagrams(strs []string) [][]string {
	anagramMap := make(map[string][]string)

	for _, str := range strs {
		// Sort the characters in the string
		sortedStr := sortString(str)

		// Check if the sorted string exists as a key in the map
		if _, found := anagramMap[sortedStr]; found {
			anagramMap[sortedStr] = append(anagramMap[sortedStr], str)
		} else {
			anagramMap[sortedStr] = []string{str}
		}
	}

	// Convert the map values to a 2D slice
	result := make([][]string, 0, len(anagramMap))
	for _, v := range anagramMap {
		result = append(result, v)
	}

	return result
}

func sortString(s string) string {
	chars := strings.Split(s, "")
	sort.Strings(chars)
	return strings.Join(chars, "")
}

func main() {
	strs := []string{"eat", "tea", "tan", "ate", "nat", "bat"}

	groups := groupAnagrams(strs)
	fmt.Println(groups)
}

```

In the groupAnagrams function, we create a hashmap anagramMap where the key is the sorted version of each string, and the value is a slice of strings that are anagrams of each other. We iterate over the input strs slice and for each string, we sort its characters using the sortString helper function. We then check if the sorted string already exists as a key in the map. If it does, we append the string to the corresponding value slice. If it doesn't, we create a new entry in the map with the sorted string as the key and the string as the value.

Finally, we convert the values of the anagramMap to a 2D slice result and return it.

In the main function, we provide an example input strs slice and call the groupAnagrams function. The result is a 2D slice where each inner slice represents a group of anagrams. We print the result to the console.

#### Top K Frequent Elements

```
package main

import (
	"fmt"
	"sort"
)

func topKFrequent(nums []int, k int) []int {
	frequencyMap := make(map[int]int)

	// Count the frequency of each number
	for _, num := range nums {
		frequencyMap[num]++
	}

	// Create a slice of the unique numbers
	uniqueNums := make([]int, 0, len(frequencyMap))
	for num := range frequencyMap {
		uniqueNums = append(uniqueNums, num)
	}

	// Sort the unique numbers based on their frequency in descending order
	sort.Slice(uniqueNums, func(i, j int) bool {
		return frequencyMap[uniqueNums[i]] > frequencyMap[uniqueNums[j]]
	})

	// Get the top K frequent elements
	result := uniqueNums[:k]

	return result
}

func main() {
	nums := []int{1, 1, 1, 2, 2, 3}
	k := 2

	topK := topKFrequent(nums, k)
	fmt.Println(topK)
}

```
In the topKFrequent function, we create a hashmap frequencyMap to count the frequency of each number in the nums slice. We then create a slice uniqueNums to store the unique numbers from the nums slice.

Next, we sort the uniqueNums slice based on the frequency of each number in descending order. We do this by using the sort.Slice function and providing a custom comparison function.

Finally, we return the top k frequent elements by slicing the uniqueNums slice from index 0 to k.

In the main function, we provide an example input nums slice and k value. We call the topKFrequent function to find the top k frequent elements and print the result to the console.


#### Product of Array Except Self

```
package main

import "fmt"

func productExceptSelf(nums []int) []int {
	n := len(nums)
	result := make([]int, n)

	// Calculate the product of all elements to the left of each index
	leftProduct := 1
	for i := 0; i < n; i++ {
		result[i] = leftProduct
		leftProduct *= nums[i]
	}

	// Calculate the product of all elements to the right of each index and multiply with the left product
	rightProduct := 1
	for i := n - 1; i >= 0; i-- {
		result[i] *= rightProduct
		rightProduct *= nums[i]
	}

	return result
}

func main() {
	nums := []int{1, 2, 3, 4, 5}

	result := productExceptSelf(nums)
	fmt.Println(result)
}

```
In the productExceptSelf function, we create a result slice with the same length as the input nums slice. We initialize a variable leftProduct to store the running product of all elements to the left of each index. We iterate through the nums slice from left to right and store the left product at each index in the result slice.

Next, we calculate the product of all elements to the right of each index and multiply it with the corresponding left product stored in the result slice. We use a variable rightProduct to keep track of the running product from right to left. We iterate through the nums slice from right to left and update the result slice by multiplying the right product with the left product.

Finally, we return the resulting product array.

In the main function, we provide an example input nums slice. We call the productExceptSelf function to calculate the product of array except self and store the result in the result slice. Finally, we print the result to the console.


#### Encode and Decode Strings

```
package main

import (
	"fmt"
	"strconv"
	"strings"
)

func encode(strings []string) string {
	result := ""
	for _, str := range strings {
		lenStr := strconv.Itoa(len(str))
		result += lenStr + "#" + str
	}
	return result
}

func decode(str string) []string {
	result := []string{}
	strs := strings.Split(str, "#")
	i := 0
	for i < len(strs)-1 {
		lenStr, _ := strconv.Atoi(strs[i])
		result = append(result, strs[i+1][:lenStr])
		i += 2
	}
	return result
}

func main() {
	strings := []string{"hello", "world", "example"}

	encoded := encode(strings)
	fmt.Println("Encoded:", encoded)

	decoded := decode(encoded)
	fmt.Println("Decoded:", decoded)
}


```
In the encode function, we iterate through the input strings slice and for each string, we calculate its length and append it to the result string along with a "#" separator, followed by the actual string. This way, we encode each string as "<length>#<string>".

In the decode function, we split the input str by the "#" separator, resulting in an array of string segments. We iterate through the segments in pairs, where the first segment represents the length of the string and the second segment represents the encoded string. We convert the length to an integer and extract the corresponding substring from the encoded string. This substring is added to the result slice.

In the main function, we provide an example input strings slice. We call the encode function to encode the strings and store the result in the encoded variable. We then print the encoded string. Next, we call the decode function to decode the encoded string and store the result in the decoded variable. Finally, we print the decoded strings to the console


#### Longest Consecutive Sequence

```
package main

import (
	"fmt"
)

func longestConsecutive(nums []int) int {
	numSet := make(map[int]bool)
	longest := 0

	// Add all numbers to the set
	for _, num := range nums {
		numSet[num] = true
	}

	// Iterate through each number in the set
	for num := range numSet {
		// Check if the current number is the start of a sequence
		if !numSet[num-1] {
			currNum := num
			currStreak := 1

			// Find the length of the consecutive sequence
			for numSet[currNum+1] {
				currNum++
				currStreak++
			}

			// Update the longest streak if necessary
			if currStreak > longest {
				longest = currStreak
			}
		}
	}

	return longest
}

func main() {
	nums := []int{100, 4, 200, 1, 3, 2, 5}

	longest := longestConsecutive(nums)
	fmt.Println("Longest Consecutive Sequence:", longest)
}

```
In the longestConsecutive function, we use a hashmap (numSet) to store all the numbers from the input nums slice. We iterate through each number in nums and add it to the hashmap. This allows us to quickly check if a number exists in the set with constant time complexity.

Next, we iterate through each number in the numSet hashmap. For each number, we check if it is the start of a sequence by verifying if the previous number (num-1) exists in the hashmap. If it does not exist, we know that the current number is the start of a consecutive sequence.

We then increment currNum and currStreak to find the length of the consecutive sequence. We continue incrementing currNum and updating currStreak as long as the next consecutive number (currNum+1) exists in the hashmap.

After finding the length of the current consecutive sequence, we update the longest variable if the current streak is longer than the previous longest streak.

Finally, we return the longest value, which represents the length of the longest consecutive sequence in the input nums slice.

In the main function, we provide an example input nums slice. We call the longest consecutive function to calculate the length of the longest consecutive sequence and store the result in the longest variable. Finally, we print the longest consecutive sequence to the console.


