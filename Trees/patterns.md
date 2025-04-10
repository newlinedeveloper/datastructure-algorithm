### Tree patterns

---

### 🌲 **1. Depth-First Search (DFS) Traversal**
- **Variants**: Preorder, Inorder, Postorder
- **Use Cases**: Tree traversals, expression evaluation, subtree calculations
- **Examples**:
  - Inorder traversal of a binary tree
  - Postorder to delete a tree
  - Preorder to clone a tree

---

### 🔁 **2. Breadth-First Search (BFS) / Level Order Traversal**
- Uses a **queue**
- **Use Cases**: Level-wise operations, finding shortest path in unweighted trees
- **Examples**:
  - Level order traversal
  - Right/Left view of the tree
  - Zigzag level order traversal

---

### 📏 **3. Diameter / Height of Tree**
- Involves **post-order DFS** and recursive height tracking
- **Use Cases**: Longest path, tree depth
- **Examples**:
  - Diameter of binary tree
  - Height-balanced tree check (AVL tree condition)

---

### 🔍 **4. Path Finding / Path Sum**
- **Variants**: Root to leaf, any-to-any path
- **Use Cases**: Backtracking + DFS to explore paths
- **Examples**:
  - Path sum equal to target
  - Print all root-to-leaf paths
  - Maximum path sum in binary tree

---

### 🔁 **5. Recursion + Divide and Conquer**
- Break down tree into subtrees, solve, and merge
- **Examples**:
  - Lowest common ancestor (LCA)
  - Invert binary tree
  - Merge two binary trees

---

### 🏷️ **6. Constructing Trees**
- From traversal arrays (Inorder, Preorder, Postorder)
- Requires mapping + recursion
- **Examples**:
  - Build tree from Inorder & Preorder/Postorder
  - Serialize and deserialize binary tree

---

### 📐 **7. Binary Search Tree (BST) Properties**
- Leverage BST properties: `left < root < right`
- **Examples**:
  - Validate BST
  - Find kth smallest/largest element
  - Convert BST to sorted DLL or Balanced BST

---

### 🧠 **8. Tree Dynamic Programming (Tree DP)**
- Store info from child nodes and compute parent
- **Examples**:
  - Maximum sum of non-adjacent nodes
  - Robbing houses in binary tree (House Robber III)
  - Counting number of nodes with a given property

---

### 🔁 **9. Morris Traversal (Inorder without recursion/stack)**
- **Use Case**: Inorder traversal in O(1) space
- **Example**: Efficient space traversal for large trees

---

### 🧭 **10. LCA (Lowest Common Ancestor)**
- Key for finding common points in paths
- **Methods**:
  - Parent tracking + path comparison
  - DFS with recursion and subtree checks

---

Perfect! Here's a deep-dive into **Tree DSA Problem Patterns**, including:

- 🔍 How to **identify** the pattern  
- 🛠️ How to **solve** it (approach/strategy)  
- 💡 **Common problems** for each pattern  

---

## 🌲 1. **DFS Traversal (Preorder, Inorder, Postorder)**

### 🔍 Identification:
- Questions mention **traversing** every node in **a specific order**.
- Often recursive or asks for a list of values in that order.

### 🛠️ How to solve:
- Use recursion (preferred) or stack (iterative)
- **Inorder (LNR)**, **Preorder (NLR)**, **Postorder (LRN)**

### 💡 Problems:
- [✓] Inorder Traversal – [Leetcode #94](https://leetcode.com/problems/binary-tree-inorder-traversal/)
- [✓] Preorder Traversal – [Leetcode #144](https://leetcode.com/problems/binary-tree-preorder-traversal/)
- [✓] Postorder Traversal – [Leetcode #145](https://leetcode.com/problems/binary-tree-postorder-traversal/)

---

## 🔁 2. **BFS / Level Order Traversal**

### 🔍 Identification:
- You’re asked to process nodes **level by level**.
- May involve **left/right view**, **zigzag**, or **level sums**.

### 🛠️ How to solve:
- Use a **queue**
- Track levels using **size of the queue** or pair nodes with level number

### 💡 Problems:
- [✓] Binary Tree Level Order – [Leetcode #102](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [✓] Zigzag Level Order – [Leetcode #103](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
- [✓] Right Side View – [Leetcode #199](https://leetcode.com/problems/binary-tree-right-side-view/)

---

## 📏 3. **Diameter / Height / Depth of Tree**

### 🔍 Identification:
- The problem asks for **height**, **depth**, or **longest path**.
- Involves combining height of left and right subtrees.

### 🛠️ How to solve:
- Use **post-order DFS**
- Return height; while backtracking, update diameter or path

### 💡 Problems:
- [✓] Diameter of Binary Tree – [Leetcode #543](https://leetcode.com/problems/diameter-of-binary-tree/)
- [✓] Balanced Binary Tree – [Leetcode #110](https://leetcode.com/problems/balanced-binary-tree/)
- [✓] Maximum Depth – [Leetcode #104](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

---

## 🔍 4. **Path Finding / Path Sum**

### 🔍 Identification:
- Problem involves **finding paths from root to leaf** or **specific path sums**.
- Might ask for “Does a path exist?”, “How many such paths?”, etc.

### 🛠️ How to solve:
- Use DFS/backtracking
- Track current path and sum as you go

### 💡 Problems:
- [✓] Path Sum – [Leetcode #112](https://leetcode.com/problems/path-sum/)
- [✓] Path Sum II (return all paths) – [Leetcode #113](https://leetcode.com/problems/path-sum-ii/)
- [✓] Binary Tree Maximum Path Sum – [Leetcode #124](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

---

## 🧠 5. **Recursion + Divide & Conquer**

### 🔍 Identification:
- Combine results from **left and right** subtrees to solve a problem.
- Often used in subtree problems or combining trees.

### 🛠️ How to solve:
- Use recursion to split the tree
- Solve for left and right children, then combine result

### 💡 Problems:
- [✓] Lowest Common Ancestor – [Leetcode #236](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
- [✓] Invert Binary Tree – [Leetcode #226](https://leetcode.com/problems/invert-binary-tree/)
- [✓] Merge Two Binary Trees – [Leetcode #617](https://leetcode.com/problems/merge-two-binary-trees/)

---

## 🧱 6. **Construct Trees from Traversals**

### 🔍 Identification:
- You’re given **inorder + preorder/postorder**, and asked to build the tree.
- Always unique tree construction.

### 🛠️ How to solve:
- Use recursion
- Inorder helps divide left/right subtrees
- Pre/Post order gives you the root node

### 💡 Problems:
- [✓] Construct from Preorder + Inorder – [Leetcode #105](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- [✓] Construct from Postorder + Inorder – [Leetcode #106](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

---

## 📊 7. **BST (Binary Search Tree) Properties**

### 🔍 Identification:
- Tree is a **BST**, or you’re told to **validate**, **search**, or **insert**.
- Leverage **sorted order**: `left < root < right`.

### 🛠️ How to solve:
- Use recursion
- Use min/max range to validate
- BST properties to search efficiently

### 💡 Problems:
- [✓] Validate BST – [Leetcode #98](https://leetcode.com/problems/validate-binary-search-tree/)
- [✓] Kth Smallest Element – [Leetcode #230](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
- [✓] Convert Sorted Array to BST – [Leetcode #108](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

---

## 🧮 8. **Tree Dynamic Programming (Tree DP)**

### 🔍 Identification:
- Tree + **optimization problem** (e.g., max/min values, choices at each node)
- Can't choose both parent and child at once, etc.

### 🛠️ How to solve:
- Use **post-order traversal**
- Store and combine results from children

### 💡 Problems:
- [✓] House Robber III – [Leetcode #337](https://leetcode.com/problems/house-robber-iii/)
- [✓] Count Good Nodes – [Leetcode #1448](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)
- [✓] Longest Univalue Path – [Leetcode #687](https://leetcode.com/problems/longest-univalue-path/)

---

## 💾 9. **Morris Traversal (Space-Optimized Traversal)**

### 🔍 Identification:
- Asked to do **inorder traversal without recursion/stack**
- Optimize space to **O(1)**

### 🛠️ How to solve:
- Use **threaded binary tree**
- Temporarily link nodes to their predecessor

### 💡 Problems:
- [✓] Inorder Morris Traversal (custom implementation)
- Used in **flattening trees** or space-optimized traversals

---

## 📍 10. **Lowest Common Ancestor (LCA)**

### 🔍 Identification:
- Asked to find **lowest common ancestor** of two nodes
- Tree is often **binary** or **BST**

### 🛠️ How to solve:
- Use recursion:
  - If current node is p or q, return node
  - Recurse left and right
  - If both sides return non-null → current node is LCA

### 💡 Problems:
- [✓] LCA in Binary Tree – [Leetcode #236](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
- [✓] LCA in BST – [Leetcode #235](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

----


## 🔁 1. **Recursive Traversal (DFS – Preorder, Inorder, Postorder)**

### 🧠 Why Google likes it:
- Tests recursion, base cases, tree understanding.

### 📌 Problems:
- ✅ **Binary Tree Inorder Traversal** – Leetcode #94 *(recursive & iterative approach)*
- ✅ **Binary Tree Postorder Traversal** – Leetcode #145
- ✅ **Flatten Binary Tree to Linked List** – Leetcode #114 *(Google favorite)*

---

## 📶 2. **Level Order Traversal (BFS)**

### 🧠 Why Google likes it:
- Good for graph and tree crossover concepts, layering logic.

### 📌 Problems:
- ✅ **Binary Tree Level Order Traversal** – Leetcode #102
- ✅ **Binary Tree Right Side View** – Leetcode #199
- ✅ **Populating Next Right Pointers in Each Node** – Leetcode #116

---

## 📐 3. **Tree Height / Diameter / Balance Check**

### 🧠 Why Google likes it:
- Requires bottom-up recursion, combines tree metrics.

### 📌 Problems:
- ✅ **Diameter of Binary Tree** – Leetcode #543
- ✅ **Balanced Binary Tree** – Leetcode #110
- ✅ **Minimum Depth of Binary Tree** – Leetcode #111

---

## 📂 4. **Tree Path Problems (Path Sum, Path Finding)**

### 🧠 Why Google likes it:
- Emphasizes DFS, backtracking, managing state in recursion.

### 📌 Problems:
- ✅ **Path Sum** – Leetcode #112
- ✅ **Path Sum II** – Leetcode #113
- ✅ **Binary Tree Maximum Path Sum** – Leetcode #124 *(Common Google bar-raiser)*

---

## 🧩 5. **Lowest Common Ancestor (LCA)**

### 🧠 Why Google likes it:
- Combines tree traversal, understanding ancestor relationships.

### 📌 Problems:
- ✅ **LCA in Binary Tree** – Leetcode #236
- ✅ **LCA in BST** – Leetcode #235
- ✅ **Step-by-step LCA with Path Tracking** *(custom variant often asked)*

---

## 💡 6. **BST-Specific Logic**

### 🧠 Why Google likes it:
- Tests knowledge of BST properties and efficient search.

### 📌 Problems:
- ✅ **Validate BST** – Leetcode #98
- ✅ **Kth Smallest in BST** – Leetcode #230 *(Google has asked this with augmented BST)*
- ✅ **Lowest Common Ancestor in BST** – Leetcode #235

---

## 🧠 7. **Construct Tree from Traversals**

### 🧠 Why Google likes it:
- Requires thinking in recursive decomposition and array slicing.

### 📌 Problems:
- ✅ **Build Tree from Preorder & Inorder** – Leetcode #105
- ✅ **Build Tree from Inorder & Postorder** – Leetcode #106

---

## 🔧 8. **Serialization / Deserialization**

### 🧠 Why Google likes it:
- Combines recursion, data structures, string parsing – good for system design lite.

### 📌 Problems:
- ✅ **Serialize and Deserialize Binary Tree** – Leetcode #297 *(frequently asked)*

---

## 🧮 9. **Tree Dynamic Programming (Tree DP)**

### 🧠 Why Google likes it:
- Tree DP requires memoization + recursion + subtree aggregation.

### 📌 Problems:
- ✅ **House Robber III** – Leetcode #337
- ✅ **Longest Univalue Path** – Leetcode #687
- ✅ **Count Good Nodes in Binary Tree** – Leetcode #1448

---

## 💥 10. **Advanced / Unique Tree Structures**

### 🧠 Why Google likes it:
- Requires custom traversal or tricky logic.

### 📌 Problems:
- ✅ **Recover Binary Search Tree** – Leetcode #99 *(detect and fix swapped nodes)*
- ✅ **Binary Tree Cameras** – Leetcode #968 *(Google bar-raiser)*
- ✅ **Boundary of Binary Tree** – Leetcode #545

---

## 📈 Bonus: Graph + Tree Crossover

### 🧠 Why Google likes it:
- Tests how well you generalize from tree to graph.

### 📌 Problems:
- ✅ **Reconstruct Itinerary** – Leetcode #332 *(tree-like DFS on graph)*
- ✅ **Clone N-ary Tree** – Similar to clone graph

---

## 🎯 How to Prepare for Google Tree Questions

| Step | Strategy |
|------|----------|
| 🔍 Identify | Check if it’s binary, BST, or N-ary. |
| 🔄 Recursion | Most tree problems can be solved using DFS recursion. |
| 🧮 Combine results | For Tree DP, return tuples from children. |
| 📦 Track state | Use global variables carefully for max/min/path tracking. |
| 💬 Practice explaining | In interviews, **explain your base case and recursion clearly**. |

---
