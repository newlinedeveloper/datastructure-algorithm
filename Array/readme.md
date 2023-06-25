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

#### Encode and Decode Strings

#### Longest Consecutive Sequence
