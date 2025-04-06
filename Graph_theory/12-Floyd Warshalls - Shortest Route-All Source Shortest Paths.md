Absolutely! Let’s go through **Floyd-Warshall’s Algorithm** using the problem:

---

## 🧩 **CSES 1672 – Shortest Routes II**

🔗 [Problem Link](https://cses.fi/problemset/task/1672)

---

## 📘 **Problem Summary**

You are given a **directed weighted graph** with `n` nodes and `m` edges.  
You must answer `q` queries. Each query asks:  
👉 What is the **shortest distance** from node `a` to node `b`?

---

## ⚙️ **Constraints**

- `1 ≤ n ≤ 500` — small enough for `O(n³)`
- `1 ≤ m ≤ 10⁵`
- `1 ≤ q ≤ 10⁵`
- Edge weights can be positive or zero (but not negative)

---

## ✅ **Goal**

For **every pair** of nodes, compute the shortest distance.  
Use **Floyd-Warshall Algorithm** — the classic **All-Pairs Shortest Path** (APSP) algorithm.

---

## 🚀 **Floyd-Warshall Algorithm Overview**

### Idea:

For each pair `(i, j)`, check if going through an intermediate node `k` gives a shorter path.

### Formula:

```go
if dist[i][j] > dist[i][k] + dist[k][j] {
	dist[i][j] = dist[i][k] + dist[k][j]
}
```

- Repeat for all `k = 1..n`
- Then for all `i = 1..n`, and all `j = 1..n`

---

## ✅ **Go Implementation**

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

const INF int64 = 1e18

func main() {
	reader := bufio.NewReader(os.Stdin)
	writer := bufio.NewWriter(os.Stdout)
	defer writer.Flush()

	var n, m, q int
	fmt.Fscan(reader, &n, &m, &q)

	// Initialize distance matrix
	dist := make([][]int64, n+1)
	for i := range dist {
		dist[i] = make([]int64, n+1)
		for j := range dist[i] {
			if i == j {
				dist[i][j] = 0
			} else {
				dist[i][j] = INF
			}
		}
	}

	// Read edges
	for i := 0; i < m; i++ {
		var a, b int
		var w int64
		fmt.Fscan(reader, &a, &b, &w)
		if w < dist[a][b] {
			dist[a][b] = w
		}
	}

	// Floyd-Warshall
	for k := 1; k <= n; k++ {
		for i := 1; i <= n; i++ {
			for j := 1; j <= n; j++ {
				if dist[i][k] < INF && dist[k][j] < INF {
					if dist[i][j] > dist[i][k]+dist[k][j] {
						dist[i][j] = dist[i][k] + dist[k][j]
					}
				}
			}
		}
	}

	// Answer queries
	for i := 0; i < q; i++ {
		var a, b int
		fmt.Fscan(reader, &a, &b)
		if dist[a][b] >= INF {
			fmt.Fprintln(writer, -1)
		} else {
			fmt.Fprintln(writer, dist[a][b])
		}
	}
}
```

---

## 🔢 Sample Input

```
4 4 3
1 2 5
2 3 2
1 3 9
3 4 1
1 3
1 4
4 1
```

### ✅ Sample Output

```
7
8
-1
```

---

## ⏱ **Time Complexity**

| Step                | Complexity       |
|---------------------|------------------|
| Floyd-Warshall      | O(n³) = ~125M ops (max) |
| Query answering     | O(1) per query   |
| Total               | Efficient for `n ≤ 500` |

---

## 🧠 Notes

- Works with positive and zero-weight edges.
- Doesn’t work with **negative cycles** (but CSES problem guarantees **no negative weights**).
- Perfect for **dense graphs** with **many queries**.

---
