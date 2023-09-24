### Tree

#### Problem 1 : Invert Binary Tree -  Easy

```
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }
    root.Left, root.Right = invertTree(root.Right), invertTree(root.Left)
    return root
}

```

This code defines a TreeNode struct with an integer value, a left child node, and a right child node. The invertTree function takes a root node as its argument and recursively inverts the tree by swapping the left and right child nodes. Finally, the function returns the root node of the inverted tree.

Here's an example usage of the function:


```
func main() {
    root := &TreeNode{Val: 4, Left: &TreeNode{Val: 2, Left: &TreeNode{Val: 1}, Right: &TreeNode{Val: 3}}, Right: &TreeNode{Val: 7, Left: &TreeNode{Val: 6}, Right: &TreeNode{Val: 9}}}
    fmt.Println("Before inversion:")
    printTree(root)
    inverted := invertTree(root)
    fmt.Println("After inversion:")
    printTree(inverted)
}

func printTree(root *TreeNode) {
    if root == nil {
        return
    }
    fmt.Println(root.Val)
    printTree(root.Left)
    printTree(root.Right)
}


```
This code creates a binary tree with the values [4, 2, 7, 1, 3, 6, 9], prints the tree before and after inversion using a helper function printTree, and outputs the following:

```

Before inversion:
4
2
1
3
7
6
9
After inversion:
4
7
9
6
2
3
1

```



#### Problem 2 : Maximum Depth of Binary Tree -  Easy

```
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func maxDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }
    leftDepth := maxDepth(root.Left)
    rightDepth := maxDepth(root.Right)
    if leftDepth > rightDepth {
        return leftDepth + 1
    } else {
        return rightDepth + 1
    }
}


```

This code defines a TreeNode struct with an integer value, a left child node, and a right child node. The maxDepth function takes a root node as its argument and recursively calculates the maximum depth of the tree by finding the maximum depth of the left and right subtrees and adding 1 for the root node. The function returns the maximum depth of the tree.

Here's an example usage of the function:


```
func main() {
    root := &TreeNode{Val: 3, Left: &TreeNode{Val: 9}, Right: &TreeNode{Val: 20, Left: &TreeNode{Val: 15}, Right: &TreeNode{Val: 7}}}
    depth := maxDepth(root)
    fmt.Println("Maximum depth of the binary tree is:", depth)
}


```
This code creates a binary tree with the values [3, 9, 20, null, null, 15, 7], finds the maximum depth of the tree using the maxDepth function, and outputs the following:

```
Maximum depth of the binary tree is: 3

```



#### Problem 3 : Diameter of Binary Tree -  Easy


```
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func diameterOfBinaryTree(root *TreeNode) int {
    if root == nil {
        return 0
    }
    leftHeight := height(root.Left)
    rightHeight := height(root.Right)
    leftDiameter := diameterOfBinaryTree(root.Left)
    rightDiameter := diameterOfBinaryTree(root.Right)
    return max(leftHeight+rightHeight, max(leftDiameter, rightDiameter))
}

func height(root *TreeNode) int {
    if root == nil {
        return 0
    }
    return 1 + max(height(root.Left), height(root.Right))
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}


```
This code defines a TreeNode struct with an integer value, a left child node, and a right child node. The diameterOfBinaryTree function takes a root node as its argument and recursively calculates the diameter of the tree by finding the heights of the left and right subtrees, the diameter of the left and right subtrees, and returning the maximum of the sum of the heights and the diameters of the subtrees. The height function is a helper function that calculates the height of a tree rooted at the given node. The max function is a helper function that returns the maximum of two integers.

Here's an example usage of the function:

```
func main() {
    root := &TreeNode{Val: 1, Left: &TreeNode{Val: 2, Left: &TreeNode{Val: 4}, Right: &TreeNode{Val: 5}}, Right: &TreeNode{Val: 3}}
    diameter := diameterOfBinaryTree(root)
    fmt.Println("Diameter of the binary tree is:", diameter)
}


```

This code creates a binary tree with the values [1, 2, 3, 4, 5], finds the diameter of the tree using the diameterOfBinaryTree function, and outputs the following:

```
Diameter of the binary tree is: 3


```

#### Problem 4 : Diameter of Binary Tree -  Easy


```

package main

import "fmt"

type Node struct {
    Left  *Node
    Right *Node
}

func diameter(root *Node) int {
    if root == nil {
        return 0
    }

    // calculate the diameter of the left and right subtrees recursively
    leftDiameter := diameter(root.Left)
    rightDiameter := diameter(root.Right)

    // calculate the height of the left and right subtrees recursively
    leftHeight := height(root.Left)
    rightHeight := height(root.Right)

    // return the maximum of the following three values:
    // 1) diameter of the left subtree
    // 2) diameter of the right subtree
    // 3) the sum of the heights of the left and right subtrees plus 1 (for the current node)
    return max(leftDiameter, max(rightDiameter, leftHeight+rightHeight+1))
}

func height(node *Node) int {
    if node == nil {
        return 0
    }
    // calculate the height of the left and right subtrees recursively
    leftHeight := height(node.Left)
    rightHeight := height(node.Right)

    // return the maximum height of the left and right subtrees plus 1 (for the current node)
    return max(leftHeight, rightHeight) + 1
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func main() {
    root := &Node{
        Left: &Node{
            Left: &Node{},
            Right: &Node{
                Left:  &Node{},
                Right: &Node{},
            },
        },
        Right: &Node{},
    }

    fmt.Println(diameter(root)) // output: 5
}


```


In this implementation, we define a Node struct to represent the nodes of the binary tree. The diameter function takes a root node as input and recursively calculates the diameter of the binary tree using the height function. The height function calculates the height of the binary tree recursively. The max function returns the maximum of two integers. In the main function, we create a sample binary tree and call the diameter function to find its diameter.

#### Problem 5 : Diameter of Binary Tree -  Easy

```
package main

import (
	"fmt"
	"math"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func diameterOfBinaryTree(root *TreeNode) int {
	diameter := 0
	calculateDepth(root, &diameter)
	return diameter
}

func calculateDepth(node *TreeNode, diameter *int) int {
	if node == nil {
		return 0
	}

	leftDepth := calculateDepth(node.Left, diameter)
	rightDepth := calculateDepth(node.Right, diameter)

	// Calculate the current diameter
	*(*diameter) = int(math.Max(float64(*diameter), float64(leftDepth+rightDepth)))

	// Return the depth of the current node
	return int(math.Max(float64(leftDepth), float64(rightDepth))) + 1
}

func main() {
	// Create a sample binary tree
	root := &TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Right = &TreeNode{Val: 3}
	root.Left.Left = &TreeNode{Val: 4}
	root.Left.Right = &TreeNode{Val: 5}

	// Calculate the diameter of the binary tree
	diameter := diameterOfBinaryTree(root)
	fmt.Println("Diameter of Binary Tree:", diameter)
}

```
In this code, we define a TreeNode struct to represent a node in the binary tree. Each TreeNode has a Val field to store the value and Left and Right pointers to its left and right child nodes, respectively.

The diameterOfBinaryTree function calculates the diameter of the binary tree by recursively traversing the tree and updating the diameter variable. It calls the calculateDepth function, which calculates the depth of each node in the binary tree and updates the diameter if necessary.

The calculateDepth function recursively calculates the depth of each node by calling itself on the left and right subtrees. It uses the math.Max function to compare the left and right depths and update the diameter if the sum of the left and right depths is greater than the current diameter.

Finally, in the main function, we create a sample binary tree by creating TreeNode instances and setting their values and connections. We call the diameterOfBinaryTree function with the root node of the binary tree to calculate the diameter, and then print the result to the console.

#### Same Tree

```
package main

// Definition for a binary tree node.
type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func isSameTree(p *TreeNode, q *TreeNode) bool {
	// If both trees are nil, they are the same.
	if p == nil && q == nil {
		return true
	}

	// If one tree is nil and the other is not, they are different.
	if p == nil || q == nil {
		return false
	}

	// Check if the current nodes have the same value.
	if p.Val != q.Val {
		return false
	}

	// Recursively check the left and right subtrees.
	return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}


```

The TreeNode struct represents a node in a binary tree with an integer value, a left child node, and a right child node.

The isSameTree function takes two binary trees p and q as input and returns a boolean indicating whether they are the same.

If both p and q are nil, it means the current subtrees are the same, so we return true.

If one of p or q is nil while the other is not, it means the trees have different structures, so we return false.

If neither p nor q is nil, we check if the values of the current nodes (p.Val and q.Val) are the same. If they are not equal, the trees are different, and we return false.

Finally, we recursively call the isSameTree function on the left subtrees (p.Left and q.Left) and the right subtrees (p.Right and q.Right) to check if they are the same.

By following this recursive approach, the function will traverse both trees, comparing their values and structures. If the traversal completes without finding any differences, it returns true, indicating that the two trees are the same; otherwise, it returns false.

You can call this function with two binary tree roots to determine if they are the same or not.

#### Subtree of another tree

```
package main

// Definition for a binary tree node.
type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func isSubtree(root *TreeNode, subRoot *TreeNode) bool {
	if root == nil {
		return false // If the main tree is empty, no subtree can exist.
	}
	// Check if the current subtree rooted at `root` is the same as `subRoot`.
	if isSameTree(root, subRoot) {
		return true
	}
	// If not, recursively check the left and right subtrees of `root`.
	return isSubtree(root.Left, subRoot) || isSubtree(root.Right, subRoot)
}

func isSameTree(p *TreeNode, q *TreeNode) bool {
	if p == nil && q == nil {
		return true // Both nodes are nil, they match.
	}
	if p == nil || q == nil {
		return false // One node is nil but the other is not, they don't match.
	}
	if p.Val != q.Val {
		return false // Node values are different, they don't match.
	}
	// Recursively check left and right subtrees.
	return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}

```









