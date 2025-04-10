## 🔁 1. **Fibonacci / Recurrence Pattern**
> Use when the current solution depends on a fixed number of previous solutions.

| Problem Name                        | Platform    | Difficulty |
|------------------------------------|-------------|------------|
| Fibonacci Number                   | LeetCode #509     | Easy       |
| Climbing Stairs                    | LeetCode #70      | Easy       |
| Tribonacci Number                  | LeetCode #1137    | Easy       |
| Number of Ways to Reach the End    | HackerRank         | Medium     |
| Decode Ways                        | LeetCode #91      | Medium     |

---

## 🧳 2. **0/1 Knapsack Pattern**
> Make binary choices (include or exclude) with constraints like weight or capacity.

| Problem Name                        | Platform    | Difficulty |
|------------------------------------|-------------|------------|
| Subset Sum                         | GeeksForGeeks / LeetCode Discuss | Medium     |
| Partition Equal Subset Sum         | LeetCode #416    | Medium     |
| Target Sum                         | LeetCode #494    | Medium     |
| 0/1 Knapsack                       | HackerRank        | Medium     |
| Count of Subset Sum                | GeeksForGeeks     | Medium     |

---

## 💰 3. **Unbounded Knapsack Pattern**
> You can use the same item multiple times.

| Problem Name                        | Platform    | Difficulty |
|------------------------------------|-------------|------------|
| Coin Change                        | LeetCode #322    | Medium     |
| Coin Change II                     | LeetCode #518    | Medium     |
| Rod Cutting                        | GeeksForGeeks     | Medium     |
| Minimum Cost to Reach Destination | GeeksForGeeks     | Medium     |

---

## 📏 4. **Longest Common Subsequence (LCS) Pattern**
> Compare two sequences, aiming for maximum matching alignment.

| Problem Name                        | Platform    | Difficulty |
|------------------------------------|-------------|------------|
| Longest Common Subsequence         | LeetCode #1143   | Medium     |
| Longest Palindromic Subsequence    | LeetCode #516    | Medium     |
| Edit Distance                      | LeetCode #72     | Hard       |
| Minimum ASCII Delete Sum           | LeetCode #712    | Medium     |
| Shortest Common Supersequence      | LeetCode #1092   | Hard       |

---

## 🔡 5. **String Segmentation / Word Break Pattern**
> Try valid string splits based on dictionary/logic.

| Problem Name                        | Platform    | Difficulty |
|------------------------------------|-------------|------------|
| Word Break                         | LeetCode #139    | Medium     |
| Palindrome Partitioning            | LeetCode #131    | Medium     |
| Word Break II                      | LeetCode #140    | Hard       |

---

## 🧱 6. **Matrix DP Pattern**
> Navigate through a grid using optimal path logic.

| Problem Name                        | Platform    | Difficulty |
|------------------------------------|-------------|------------|
| Minimum Path Sum                   | LeetCode #64     | Medium     |
| Unique Paths                       | LeetCode #62     | Medium     |
| Unique Paths II (with obstacles)   | LeetCode #63     | Medium     |
| Maximal Square                     | LeetCode #221    | Medium     |
| Cherry Pickup                     | LeetCode #741    | Hard       |

---

## 🌳 7. **DP on Trees**
> Combine answers from child nodes in a recursive way.

| Problem Name                        | Platform    | Difficulty |
|------------------------------------|-------------|------------|
| House Robber III                   | LeetCode #337    | Medium     |
| Diameter of Binary Tree            | LeetCode #543    | Easy       |
| Binary Tree Cameras                | LeetCode #968    | Hard       |
| Maximum Path Sum                   | LeetCode #124    | Hard       |

---

## ⛓️ 8. **DP on Subsequences**
> Analyze patterns within subarrays/subsequences.

| Problem Name                        | Platform    | Difficulty |
|------------------------------------|-------------|------------|
| Longest Increasing Subsequence     | LeetCode #300    | Medium     |
| Number of Longest Increasing Subsequences | LeetCode #673 | Medium     |
| Maximum Sum Increasing Subsequence | GeeksForGeeks     | Medium     |
| Russian Doll Envelopes             | LeetCode #354    | Hard       |

---

## 🕸️ 9. **State Machine / Multi-Dimensional DP**
> Track multiple states like day, hold/sell, cooldown, or transactions.

| Problem Name                        | Platform    | Difficulty |
|------------------------------------|-------------|------------|
| Best Time to Buy and Sell Stock II | LeetCode #122    | Easy       |
| Best Time to Buy and Sell Stock with Cooldown | LeetCode #309 | Medium |
| Best Time to Buy and Sell Stock III (2 transactions) | LeetCode #123 | Hard |
| Paint House                        | LeetCode #256    | Medium     |
| Dungeon Game                       | LeetCode #174    | Hard       |
| Traveling Salesman Problem         | GeeksForGeeks     | Hard       |

----

Great question! Google is known for asking **deep, conceptual** dynamic programming (DP) problems. They typically:
- Don't **look** like DP at first glance.
- Require **pattern recognition** and strong **problem modeling**.
- Are sometimes **combinations** of multiple DP patterns (e.g., DP + Greedy, DP + Graph, DP + Recursion with Memoization).

---

## 🧠 DP Patterns Google Loves

Here’s a curated list of **DP patterns** and the **types of problems** Google has been known to ask under each one:

---

### 🔁 1. **Fibonacci-Style / Recurrence**
These are usually entry-level but form the **base for follow-ups**.

- **Example Problems:**
  - Climbing Stairs (LeetCode #70)
  - Decode Ways (LeetCode #91)
  - Ways to Make Change (like LeetCode #518 or HackerRank)
  
➡️ *Google may use these as warmups or for interns/new grads.*

---

### 🧳 2. **0/1 Knapsack & Subset Variants**
These are more complex, often wrapped in disguise.

- **Example Problems:**
  - Partition Equal Subset Sum (LeetCode #416)
  - Target Sum (LeetCode #494)
  - Subset Sum with conditions
  - Pick k items to maximize profit with constraints

➡️ *Expect questions involving choosing/not choosing elements under constraints.*

---

### 💰 3. **Unbounded Knapsack (Unlimited Choices)**
- **Example Problems:**
  - Coin Change I & II (LeetCode #322, #518)
  - Integer Break (LeetCode #343)
  - Rod Cutting (GeeksForGeeks)

➡️ *Google loves variation-rich problems where items can be reused.*

---

### 📏 4. **LCS / Edit Distance / String Matching**
Very common in interviews due to their real-world applications (e.g., spell check, DNA, diff tools).

- **Example Problems:**
  - Longest Common Subsequence (LeetCode #1143)
  - Edit Distance (LeetCode #72)
  - Regular Expression Matching (LeetCode #10)
  - Wildcard Matching (LeetCode #44)

➡️ *They may start with LCS and extend it with constraints or tweaks.*

---

### 🧱 5. **Matrix DP**
Used to test 2D thinking and tabulation.

- **Example Problems:**
  - Minimum Path Sum (LeetCode #64)
  - Unique Paths + Obstacles (LeetCode #62, #63)
  - Cherry Pickup (LeetCode #741)
  - Dungeon Game (LeetCode #174) – **classic Google**

➡️ *Watch for movement rules (only down/right), and obstacles or health constraints.*

---

### ⛓️ 6. **LIS / DP on Subsequences**
Used in optimization scenarios with ordering.

- **Example Problems:**
  - Longest Increasing Subsequence (LeetCode #300)
  - Russian Doll Envelopes (LeetCode #354)
  - Number of LIS (LeetCode #673)
  - Max Sum Increasing Subsequence

➡️ *They like to wrap these with real-world constraints (e.g., stacking envelopes, boxes).*

---

### 🧠 7. **DP + Bitmask / State Machine**
These are more advanced and often asked for senior/experienced roles.

- **Example Problems:**
  - Paint House (LeetCode #256)
  - Paint House III (LeetCode #1473) – multiple states
  - Traveling Salesman Problem (TSP)
  - Minimum Cost to Merge Stones (LeetCode #1000)
  - Boolean Parenthesization (GeeksForGeeks)

➡️ *Bitmask DP is a Google favorite when tracking combinations or permutations.*

---

### 🌳 8. **DP on Trees**
Used to test recursion + DP understanding.

- **Example Problems:**
  - House Robber III (LeetCode #337)
  - Diameter of Tree (LeetCode #543)
  - Maximum Path Sum (LeetCode #124)

➡️ *Expect post-order traversal combined with DP memoization.*

---

### 🔀 9. **DP + Greedy / Sliding Window Hybrid**
Google likes mixed patterns that test multiple skills.

- **Example Problems:**
  - Jump Game II (LeetCode #45) – DP or Greedy
  - Maximum Subarray (LeetCode #53) – Kadane’s Algo
  - Minimum Number of Refueling Stops (LeetCode #871)

➡️ *They may ask why you choose DP over greedy or vice versa.*

---

## 🔥 Common Google Interview Problems (DP-focused)

Here are some problems that have **actually been asked** in Google interviews, tagged with their pattern:

| Problem | Pattern | LeetCode ID |
|--------|---------|-------------|
| Decode Ways | Fibonacci-style | #91 |
| Edit Distance | LCS | #72 |
| Unique Paths II | Matrix DP | #63 |
| Cherry Pickup | Matrix + Multi-agent DP | #741 |
| Paint House | State Machine | #256 |
| Word Break | Word Segmentation | #139 |
| Dungeon Game | Matrix DP (min health) | #174 |
| Longest Palindromic Subsequence | LCS variant | #516 |
| Palindrome Partitioning | Backtracking + DP | #131 |
| Scramble String | Recursion + Memoization | #87 |

---

## 🧗‍♀️ Final Advice for Interviews

✅ Focus on:
- Understanding state definitions (what you’re solving at each step)
- Optimizing from recursion → memo → tabulation
- Writing clean code with helper functions
- Edge case handling

---




