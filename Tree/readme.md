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


#### Problem 3 : Diameter of Binary Tree -  Easy


#### Problem 4 : Diameter of Binary Tree -  Easy


#### Problem 5 : Diameter of Binary Tree -  Easy
