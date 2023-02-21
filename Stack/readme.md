### Stack

#### Problem 1 : Easy : Valid Parantheses

```
func isValid(s string) bool {
    stack := make([]rune, 0)

    for _, c := range s {
        if c == '(' || c == '{' || c == '[' {
            stack = append(stack, c)
        } else {
            if len(stack) == 0 {
                return false
            }
            top := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            if (c == ')' && top != '(') || (c == '}' && top != '{') || (c == ']' && top != '[') {
                return false
            }
        }
    }

    return len(stack) == 0
}


```
Here, the function isValid takes a string s and returns a boolean indicating whether the parentheses in the string are valid or not.

The function initializes a stack as a slice of runes. It then loops through each character in the string and pushes an opening parenthesis onto the stack. When it encounters a closing parenthesis, it checks whether the stack is empty. If it is, there is no matching opening parenthesis and the function returns false. If the stack is not empty, the function pops the top element from the stack and checks whether the closing parenthesis matches the top element. If it doesn't, the function returns false. The function continues to loop through the string until it has processed all characters. If there are still elements in the stack after processing the string, the parentheses are not valid and the function returns false.

If all opening parentheses have been matched with their respective closing parentheses, and the stack is empty at the end of the loop, the function returns true.

Note that this implementation uses a stack to keep track of the opening parentheses. When a closing parenthesis is encountered, the top element of the stack is checked to see if it matches. If it does not, the parentheses are not valid.


#### Problem 2 : Medium : Min Stack

```
type MinStack struct {
    stack []int
    minStack []int
}

/** initialize your data structure here. */
func Constructor() MinStack {
    return MinStack{}
}

func (this *MinStack) Push(val int) {
    this.stack = append(this.stack, val)
    if len(this.minStack) == 0 || val <= this.minStack[len(this.minStack)-1] {
        this.minStack = append(this.minStack, val)
    }
}

func (this *MinStack) Pop() {
    if this.stack[len(this.stack)-1] == this.minStack[len(this.minStack)-1] {
        this.minStack = this.minStack[:len(this.minStack)-1]
    }
    this.stack = this.stack[:len(this.stack)-1]
}

func (this *MinStack) Top() int {
    return this.stack[len(this.stack)-1]
}

func (this *MinStack) GetMin() int {
    return this.minStack[len(this.minStack)-1]
}


```

Here, the MinStack struct maintains two stacks: stack to store the elements of the stack, and minStack to store the minimum values seen so far.

The Constructor function initializes an empty MinStack. The Push function pushes an element onto the stack and onto the minStack if it is smaller than or equal to the current minimum value. The Pop function removes the top element from the stack and the minStack if they are equal. The Top function returns the top element of the stack. The GetMin function returns the minimum element seen so far, which is the top element of the minStack.

This implementation takes advantage of the fact that the minimum value in the stack only changes when a new minimum value is added, or when the current minimum value is removed. The minStack stores the minimum values in increasing order, so the minimum value seen so far is always the top element of the minStack. By keeping both stack and minStack in sync, the minimum value can be retrieved in constant time by calling GetMin().







#### Problem 3 : Medium : Evaluate Reverse Polish Notation

```
func evalRPN(tokens []string) int {
    stack := make([]int, 0)

    for _, token := range tokens {
        if token == "+" || token == "-" || token == "*" || token == "/" {
            b := stack[len(stack)-1]
            a := stack[len(stack)-2]
            stack = stack[:len(stack)-2]
            if token == "+" {
                stack = append(stack, a+b)
            } else if token == "-" {
                stack = append(stack, a-b)
            } else if token == "*" {
                stack = append(stack, a*b)
            } else {
                stack = append(stack, a/b)
            }
        } else {
            num, _ := strconv.Atoi(token)
            stack = append(stack, num)
        }
    }

    return stack[0]
}

```

Here, the function evalRPN takes a slice of strings tokens and returns an integer representing the result of the reverse polish notation expression.

The function initializes a stack as a slice of integers. It then loops through each token in the input slice. If the token is an operator, it pops the top two elements from the stack, performs the operation, and pushes the result back onto the stack. If the token is a number, it converts it to an integer using strconv.Atoi and pushes it onto the stack.

After processing all tokens, the result of the expression is the only element left in the stack. The function returns this element.

Note that this implementation uses the stack to keep track of the operands and operators in the reverse polish notation expression. When an operator is encountered, it pops the top two operands from the stack, performs the operation, and pushes the result back onto the stack. When a number is encountered, it converts it to an integer and pushes it onto the stack. By the end of the loop, the stack should contain only one element, which is the result of the expression.


#### Problem 4 : Medium : Generate Parentheses

```
func generateParenthesis(n int) []string {
    if n == 0 {
        return []string{}
    }

    result := make([]string, 0)
    stack := make([]string, 0)
    stack = append(stack, "(")
    open := 1
    close := 0

    for len(stack) > 0 {
        current := stack[len(stack)-1]
        stack = stack[:len(stack)-1]

        if len(current) == 2*n {
            result = append(result, current)
        } else {
            if open < n {
                stack = append(stack, current+"(")
                open++
            }
            if close < open {
                stack = append(stack, current+")")
                close++
            }
        }
    }

    return result
}


```

Here, the function generateParenthesis takes an integer n and returns a slice of strings representing all valid combinations of n pairs of parentheses.

The function first checks if n is zero, in which case it returns an empty slice. Otherwise, it initializes an empty slice result to store the valid combinations, and a stack stack to keep track of the current combinations being built. The stack is initially seeded with a string containing a single open parenthesis.

The function then enters a loop that continues until the stack is empty. At each iteration, it pops the top string from the stack, checks if it is a valid combination of n pairs of parentheses, and if so, appends it to the result slice. If the current string is not a valid combination, the function generates two new strings by adding an open or a close parenthesis, respectively, to the end of the current string. It then pushes these new strings onto the stack, and updates the open and close counts accordingly.

The function continues this process until all valid combinations of n pairs of parentheses have been generated, at which point it returns the result slice.

Note that this implementation uses a stack to keep track of the current combinations being built. The function generates new strings by adding an open or a close parenthesis to the end of the current string, and pushes these new strings onto the stack. By continuing this process until all valid combinations of n pairs of parentheses have been generated, the function returns a slice containing all of the valid combinations.



#### Problem 5 : Medium : Daily Temperatures

```
func dailyTemperatures(temperatures []int) []int {
    n := len(temperatures)
    result := make([]int, n)
    stack := make([]int, 0)

    for i := 0; i < n; i++ {
        for len(stack) > 0 && temperatures[i] > temperatures[stack[len(stack)-1]] {
            j := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            result[j] = i - j
        }
        stack = append(stack, i)
    }

    return result
}


```
Here, the function dailyTemperatures takes a slice of integers temperatures representing the daily temperatures and returns a slice of integers representing the number of days until the next warmer temperature.

The function first initializes a slice result of the same length as temperatures, where each element is initially set to zero. It also initializes a stack stack to keep track of the indices of temperatures that have not yet found their warmer temperature.

The function then loops through each temperature in temperatures. For each temperature, it checks if there are any temperatures on the stack that are cooler than the current temperature. If so, it pops these cooler temperatures off the stack and sets the corresponding elements of result to the difference between the indices. This represents the number of days until the next warmer temperature.

After all temperatures have been processed, the result slice contains the number of days until the next warmer temperature for each day. The function returns this slice.

Note that this implementation uses a stack to keep track of the indices of temperatures that have not yet found their warmer temperature. By maintaining this stack and popping cooler temperatures off the stack when a warmer temperature is found, the function is able to calculate the number of days until the next warmer temperature for each day.



#### Problem 6 : Medium : Car Fleet


```
type car struct {
    position int
    time     float64
}

func carFleet(target int, position []int, speed []int) int {
    n := len(position)
    cars := make([]car, n)

    for i := 0; i < n; i++ {
        cars[i] = car{position[i], float64(target-position[i]) / float64(speed[i])}
    }

    sort.Slice(cars, func(i, j int) bool {
        return cars[i].position > cars[j].position
    })

    stack := make([]float64, 0)
    for _, c := range cars {
        for len(stack) > 0 && c.time > stack[len(stack)-1] {
            stack = stack[:len(stack)-1]
        }
        stack = append(stack, c.time)
    }

    return len(stack)
}


```

Here, the function carFleet takes an integer target, a slice of integers position, and a slice of integers speed, representing the target position and starting positions and speeds of the cars, and returns the number of car fleets that will arrive at the target at the same time.

The function first creates a slice of n car structs, where each struct contains the position and time required to reach the target for a given car. It then sorts this slice by position, in descending order.

The function initializes a stack to keep track of the time required for the cars to reach the target. It then loops through each car in the sorted car slice. For each car, it pops any cars off the stack that will arrive at the target before the current car, and then pushes the time required for the current car onto the stack.

After all cars have been processed, the stack contains the time required for the cars to reach the target, in order of their position. The function returns the length of the stack, which represents the number of car fleets that will arrive at the target at the same time.

Note that this implementation uses a stack to keep track of the time required for the cars to reach the target. By maintaining this stack and popping any cars off the stack that will arrive at the target before the current car, the function is able to determine the number of car fleets that will arrive at the target at the same time.


**Sample Input & Output**

```
fmt.Println(carFleet(12, []int{10,8,0,5,3}, []int{2,4,1,1,3})) // Expected output: 3

fmt.Println(carFleet(10, []int{6,8}, []int{3,2})) // Expected output: 2

fmt.Println(carFleet(20, []int{0, 6, 11, 14}, []int{5, 4, 4, 4})) // Expected output: 2

```
In the first test case, there are 5 cars with the given positions and speeds. The expected output is 3, indicating that three car fleets will reach the target at the same time.

In the second test case, there are 2 cars with the given positions and speeds. The expected output is 2, indicating that two car fleets will reach the target at the same time.

In the third test case, there are 4 cars with the given positions and speeds. The expected output is 2, indicating that two car fleets will reach the target at the same time.



#### Problem 7 : Hard : Largest Rectangle In Histogram

```
func largestRectangleArea(heights []int) int {
    n := len(heights)
    if n == 0 {
        return 0
    }

    stack := make([]int, 0)
    maxArea := 0

    for i := 0; i <= n; i++ {
        var h int
        if i == n {
            h = 0
        } else {
            h = heights[i]
        }

        for len(stack) > 0 && h < heights[stack[len(stack)-1]] {
            height := heights[stack[len(stack)-1]]
            stack = stack[:len(stack)-1]
            var width int
            if len(stack) == 0 {
                width = i
            } else {
                width = i - stack[len(stack)-1] - 1
            }
            area := height * width
            if area > maxArea {
                maxArea = area
            }
        }

        stack = append(stack, i)
    }

    return maxArea
}



```

Here, the function largestRectangleArea takes a slice of integers heights representing the heights of bars in a histogram and returns the area of the largest rectangle that can be formed in the histogram.

The function initializes an empty stack and a variable maxArea to keep track of the maximum area seen so far. It then loops through the heights of the bars in the histogram. For each height, it checks whether the height is greater than or equal to the height of the bar at the top of the stack. If it is, it pushes the index of the current bar onto the stack. If it is not, it pops bars off the stack and calculates the area of the rectangle that can be formed by the popped bar and the bars to the left and right of it.

To calculate the width of the rectangle, the function subtracts the index of the leftmost bar in the rectangle (which is the bar at the top of the stack after the popped bar has been removed) from the index of the rightmost bar (which is the current index i). If the stack is empty after the popped bar has been removed, the leftmost bar index is assumed to be -1.

If the area of the rectangle is greater than the current maxArea, the function updates maxArea with the new area. After all bars in the histogram have been processed, the function returns maxArea, which is the area of the largest rectangle that can be formed in the histogram.

This algorithm has a time complexity of O(n) and a space complexity of O(n), where n is the number of bars in the histogram.


Here's an example of how to use the largestRectangleArea function:

```
heights := []int{2,1,5,6,2,3}
largestArea := largestRectangleArea(heights)
fmt.Println(largestArea) // Expected output: 10


```

In this example, the heights slice represents the heights of bars in a histogram. The expected output is 10, which is the area of the largest rectangle that can be formed in the histogram.




