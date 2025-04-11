Great! Let's break down **DP on Trees**, also called **"Tree DP"**.

---

## 🌳 7. DP on Trees — Combine answers from child nodes recursively

---

### 🔍 **How to Identify This Pattern**

| Clue in Problem | Pattern |
|----------------|--------|
| Binary tree / N-ary tree structure | Recursion or DFS required |
| “Max/Min value path from root to leaves” | DP over subtree |
| Need to combine results from children | Return value from each child node |
| Cannot pick both parent and child (like House Robber III) | Use multiple return values to track states |
| Ask for subtree properties (diameter, max sum, etc.) | Post-order traversal |

---

## 🟢 1. House Robber III — [LeetCode #337](https://leetcode.com/problems/house-robber-iii)

### ✅ Problem

> Each node is a house with some money. You **cannot rob directly-connected houses**. Return the **max amount** you can rob from the tree.

---

### ✅ Golang Code:

```go
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func rob(root *TreeNode) int {
    robRoot, notRobRoot := dfs(root)
    return max(robRoot, notRobRoot)
}

func dfs(node *TreeNode) (robThis, notRobThis int) {
    if node == nil {
        return 0, 0
    }

    leftRob, leftNotRob := dfs(node.Left)
    rightRob, rightNotRob := dfs(node.Right)

    robThis = node.Val + leftNotRob + rightNotRob
    notRobThis = max(leftRob, leftNotRob) + max(rightRob, rightNotRob)

    return
}

func max(a, b int) int {
    if a > b { return a }
    return b
}
```

### 🧪 Sample I/O

**Input:**
```plaintext
    3
   / \
  2   3
   \   \
    3   1
```

**Output:** `7`  
**Explanation:** Rob nodes 3 + 3 + 1.

---

## 🟢 2. Diameter of Binary Tree — [LeetCode #543](https://leetcode.com/problems/diameter-of-binary-tree)

### ✅ Problem

> Find the **length of the longest path between any two nodes** in a binary tree. The path may or may not go through the root.

---

### ✅ Golang Code:

```go
func diameterOfBinaryTree(root *TreeNode) int {
    diameter := 0

    var depth func(*TreeNode) int
    depth = func(node *TreeNode) int {
        if node == nil {
            return 0
        }

        left := depth(node.Left)
        right := depth(node.Right)

        diameter = max(diameter, left+right) // Update global max
        return max(left, right) + 1
    }

    depth(root)
    return diameter
}
```

### 🧪 Sample I/O

**Input:**
```plaintext
    1
   / \
  2   3
 / \
4   5
```

**Output:** `3`  
**Explanation:** Longest path is 4 → 2 → 1 → 3

---

## 🔴 3. Binary Tree Cameras — [LeetCode #968](https://leetcode.com/problems/binary-tree-cameras)

### ✅ Problem

> You need to place the minimum number of cameras to monitor all nodes of a binary tree. Each camera can monitor its parent, itself, and direct children.

---

### ✅ Golang Code:

```go
const (
    NOT_MONITORED = 0
    HAS_CAMERA = 1
    MONITORED = 2
)

func minCameraCover(root *TreeNode) int {
    cameras := 0

    var dfs func(*TreeNode) int
    dfs = func(node *TreeNode) int {
        if node == nil {
            return MONITORED
        }

        left := dfs(node.Left)
        right := dfs(node.Right)

        if left == NOT_MONITORED || right == NOT_MONITORED {
            cameras++
            return HAS_CAMERA
        }

        if left == HAS_CAMERA || right == HAS_CAMERA {
            return MONITORED
        }

        return NOT_MONITORED
    }

    if dfs(root) == NOT_MONITORED {
        cameras++
    }

    return cameras
}
```

### 🧪 Sample I/O

**Input:**
```plaintext
    0
   /
  0
 / \
0   0
```

**Output:** `1`  
**Explanation:** Place 1 camera at the second level.

---

## 🔴 4. Maximum Path Sum — [LeetCode #124](https://leetcode.com/problems/binary-tree-maximum-path-sum)

### ✅ Problem

> Return the maximum path sum in a binary tree. A path can start and end at any node.

---

### ✅ Golang Code:

```go
func maxPathSum(root *TreeNode) int {
    maxSum := math.MinInt

    var dfs func(*TreeNode) int
    dfs = func(node *TreeNode) int {
        if node == nil {
            return 0
        }

        left := max(dfs(node.Left), 0)
        right := max(dfs(node.Right), 0)

        current := node.Val + left + right
        maxSum = max(maxSum, current)

        return node.Val + max(left, right)
    }

    dfs(root)
    return maxSum
}
```

### 🧪 Sample I/O

**Input:**
```plaintext
   -10
   /  \
  9   20
     /  \
    15   7
```

**Output:** `42`  
**Explanation:** Max path is 15 → 20 → 7

---

## 🧠 Summary Table

| Problem | State | What is returned from DFS? | Key Idea |
|--------|-------|-----------------------------|----------|
| House Robber III | Rob / NotRob | 2 values from children | Choose max of rob vs not-rob |
| Diameter of Tree | Max path | Height of subtree | Update global max |
| Binary Tree Cameras | Coverage state | Enum: camera/covered/uncovered | Place camera if child unmonitored |
| Max Path Sum | Max path sum | Max path sum from current | Global max = left + root + right |

---
