### Sliding Window

#### Problem 1 - Easy -  Best Time to Buy & Sell stock

```
func maxProfit(prices []int) int {
    if len(prices) <= 1 {
        return 0
    }

    maxProfit := 0
    minPrice := prices[0]

    for i := 1; i < len(prices); i++ {
        if prices[i] < minPrice {
            minPrice = prices[i]
        } else {
            maxProfit = max(maxProfit, prices[i]-minPrice)
        }
    }

    return maxProfit
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}


```


Here, the function maxProfit takes an integer array prices representing the price of a stock on each day, and returns the maximum profit that can be obtained by buying the stock on one day and selling it on a later day.

The function initializes the maxProfit variable to 0 and the minPrice variable to the price on the first day. It then loops through the array, keeping track of the minimum price seen so far, and updating the maxProfit if the difference between the current price and the minimum price is greater than the current maxProfit.

The implementation takes advantage of the fact that we can't sell before we buy. Therefore, we only need to keep track of the minimum price seen so far, and we can calculate the maximum profit for each subsequent day by taking the difference between the current price and the minimum price. We update the maxProfit whenever we find a new maximum difference.

The function returns 0 if the input array has fewer than two elements, since there is no opportunity to make a profit with only one price. The max function is a helper function that returns the maximum of two integers.



#### Problem 2 - Medium: Longest substring without repeating characters

```
func lengthOfLongestSubstring(s string) int {
    n := len(s)
    maxLen := 0
    left, right := 0, 0
    charMap := make(map[byte]int)

    for right < n {
        char := s[right]
        if _, ok := charMap[char]; ok {
            // If the character is already in the map, move the left pointer
            // to the right of the last occurrence of the character.
            left = max(left, charMap[char]+1)
        }
        charMap[char] = right
        maxLen = max(maxLen, right-left+1)
        right++
    }

    return maxLen
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}


```

Here, the function lengthOfLongestSubstring takes a string s and returns the length of the longest substring without repeating characters.

The function initializes the maxLen variable to 0, the left and right pointers to the beginning of the string, and an empty charMap to keep track of the indices of each character seen so far.

The function then loops through the string, checking whether the current character is already in the charMap. If it is, the function moves the left pointer to the right of the last occurrence of the character, which ensures that the current substring contains no repeating characters. The function then updates the charMap to reflect the new occurrence of the character and updates the maxLen if the length of the current substring is greater than the previous maximum.

The function continues to move the right pointer until it reaches the end of the string, at which point the length of the longest substring without repeating characters is returned.

The max function is a helper function that returns the maximum of two integers.


#### Problem 3 - Medium: Longest repeating character replacement

```
func characterReplacement(s string, k int) int {
    n := len(s)
    maxCount := 0
    left, right := 0, 0
    charCount := make([]int, 26)

    for right < n {
        charCount[s[right]-'A']++
        maxCount = max(maxCount, charCount[s[right]-'A'])
        if right-left+1-maxCount > k {
            // If the number of characters to be replaced exceeds k,
            // move the left pointer to the right to maintain the window size.
            charCount[s[left]-'A']--
            left++
        }
        right++
    }

    return right - left
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}


```

Here, the function characterReplacement takes a string s and an integer k and returns the length of the longest substring that can be obtained by replacing at most k characters.

The function initializes the maxCount variable to 0, the left and right pointers to the beginning of the string, an array charCount to keep track of the count of each character seen so far, and a variable maxCount to keep track of the maximum count of any character seen so far.

The function then loops through the string, updating the charCount and maxCount for each character, and checking whether the number of characters that need to be replaced to obtain a substring of all repeating characters exceeds k. If it does, the function moves the left pointer to the right to maintain the window size. The function then updates the maxCount if the count of the current character is greater than the previous maximum.

The function continues to move the right pointer until it reaches the end of the string, at which point the length of the longest substring that can be obtained by replacing at most k characters is returned.

The max function is a helper function that returns the maximum of two integers.


#### Problem 4 - Hard: Minimum window substring

```
func minWindow(s string, t string) string {
    n := len(s)
    m := len(t)
    charCount := make(map[byte]int)
    requiredCount := make(map[byte]int)

    // Count the number of occurrences of each character in t
    for i := 0; i < m; i++ {
        charCount[t[i]] = 0
        requiredCount[t[i]]++
    }

    left, right := 0, 0
    formed := 0
    minLength := math.MaxInt32
    result := ""

    for right < n {
        char := s[right]
        if _, ok := charCount[char]; ok {
            charCount[char]++
            if charCount[char] == requiredCount[char] {
                formed++
            }
        }

        // If the current window contains all the characters in t,
        // move the left pointer to find the smallest window
        for formed == len(requiredCount) && left <= right {
            char := s[left]
            if _, ok := charCount[char]; ok {
                charCount[char]--
                if charCount[char] < requiredCount[char] {
                    formed--
                    length := right - left + 1
                    if length < minLength {
                        minLength = length
                        result = s[left : right+1]
                    }
                }
            }
            left++
        }

        right++
    }

    return result
}



```

Here, the function minWindow takes two strings s and t and returns the minimum window in s which will contain all the characters in t.

The function initializes two maps: charCount to keep track of the count of each character seen in the current window, and requiredCount to keep track of the required count of each character in t.

The function then loops through the string, updating the charCount and checking whether the current window contains all the characters in t. If it does, the function moves the left pointer to find the smallest window that contains all the characters in t. The function then updates the minLength and result if the length of the current window is smaller than the previous minimum.

The function continues to move the right pointer until it reaches the end of the string, at which point the minimum window that contains all the characters in t is returned.

Note that the function uses the length of the minimum window to keep track of whether all characters in t have been found. This is because the minimum window that contains all the characters in t can only be obtained when all the characters in t have been found, and once the minimum window has been found, it cannot be smaller than the previous minimum window.








