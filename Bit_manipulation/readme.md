### Bit Manipulation


#### Number of 1 Bits

```
package main

import "fmt"

func hammingWeight(num uint32) int {
	count := 0
	for num != 0 {
		count++
		num &= num - 1
	}
	return count
}

func main() {
	num := uint32(11)
	fmt.Printf("Number of 1 bits in %d: %d\n", num, hammingWeight(num))

	num = uint32(128)
	fmt.Printf("Number of 1 bits in %d: %d\n", num, hammingWeight(num))
}


```
In this code, the hammingWeight function takes an unsigned 32-bit integer num as input and returns the number of 1 bits (also known as the Hamming weight) in num.

The hammingWeight function uses the bit manipulation technique known as the Brian Kernighan's algorithm. It iteratively clears the least significant 1 bit in num by performing the bitwise AND operation with num-1. This operation effectively removes the rightmost 1 bit in each iteration. The function counts the number of iterations until num becomes 0, which gives the count of 1 bits in the original number.

In the main function, we test the hammingWeight function by calculating the number of 1 bits in two example numbers (11 and 128) and printing the results.


#### Counting Bits

```
package main

import "fmt"

func countBits(num int) []int {
	bits := make([]int, num+1)
	for i := 0; i <= num; i++ {
		bits[i] = countOnes(i)
	}
	return bits
}

func countOnes(num int) int {
	count := 0
	for num != 0 {
		count++
		num &= num - 1
	}
	return count
}

func main() {
	num := 5
	bitCounts := countBits(num)
	fmt.Printf("Number: %d\n", num)
	fmt.Printf("Bit Counts: %v\n", bitCounts)
}


```

In this code, the countBits function takes an integer num as input and returns a slice of integers representing the number of 1 bits for each number from 0 to num. It calls the countOnes function to calculate the number of 1 bits for each number.

The countOnes function is similar to the previous example for counting the number of 1 bits in a number. It uses the Brian Kernighan's algorithm to count the 1 bits by repeatedly clearing the least significant 1 bit.

In the main function, we test the countBits function by calculating the number of 1 bits for the numbers from 0 to 5 and printing the results.



#### Reverse Bits

```
package main

import "fmt"

func reverseBits(num uint32) uint32 {
	var result uint32
	for i := 0; i < 32; i++ {
		result <<= 1
		if num&1 == 1 {
			result |= 1
		}
		num >>= 1
	}
	return result
}

func main() {
	num := uint32(43261596)
	reversed := reverseBits(num)
	fmt.Printf("Number: %d\n", num)
	fmt.Printf("Reversed: %d\n", reversed)
}


```

In this code, the reverseBits function takes an unsigned 32-bit integer num as input and returns the reversed value of the bits. It uses a loop to iterate through the bits of the input number from the least significant bit to the most significant bit. The result variable is left-shifted by 1 at each iteration, and if the least significant bit of num is 1, it is set in the result using the bitwise OR operation. Finally, num is right-shifted by 1 to move to the next bit.

In the main function, we test the reverseBits function by reversing the bits of the number 43261596 and printing the results.


#### Missing Number

```
package main

import "fmt"

func missingNumber(nums []int) int {
	n := len(nums)
	missing := n

	// XOR all the numbers from 0 to n and all the numbers in the array
	// The result will be the missing number
	for i := 0; i < n; i++ {
		missing ^= i
		missing ^= nums[i]
	}

	return missing
}

func main() {
	nums := []int{3, 0, 1, 4, 2}
	missing := missingNumber(nums)
	fmt.Printf("Missing number: %d\n", missing)
}


```

In this code, the missingNumber function takes an array of integers nums as input and returns the missing number in the range from 0 to len(nums). It uses the XOR operation to find the missing number.

The idea is to XOR all the numbers from 0 to n and all the numbers in the array. XORing a number with itself will result in 0, so all the numbers that are present in the array will cancel each other out. However, the missing number will not have a corresponding number to cancel it out, so it will remain in the missing variable.

In the main function, we test the missingNumber function by finding the missing number in the array [3, 0, 1, 4, 2] and printing the result.


#### Sum of Two Integers

```
package main

import "fmt"

func getSum(a int, b int) int {
	for b != 0 {
		carry := a & b
		a = a ^ b
		b = carry << 1
	}
	return a
}

func main() {
	num1 := 5
	num2 := 7
	sum := getSum(num1, num2)
	fmt.Printf("Sum: %d\n", sum)
}


```

In this code, the getSum function takes two integers a and b as input and returns their sum using bit manipulation.

The algorithm uses a loop to iterate until there is no carry left. In each iteration, it calculates the carry by performing the bitwise AND operation between a and b. Then, it updates a by performing the bitwise XOR operation between a and b, which effectively adds the bits without considering the carry. Finally, it updates b by shifting the carry one position to the left, preparing it for the next iteration.

In the main function, we test the getSum function by calculating the sum of 5 and 7 and printing the result, which should be 12.

