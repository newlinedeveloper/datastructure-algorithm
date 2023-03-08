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

#### Problem 5 : Diameter of Binary Tree -  Easy
