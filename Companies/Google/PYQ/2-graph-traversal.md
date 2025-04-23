This problem is a **graph traversal** problem with a **time constraint**. You can model the airports as **nodes**, and each flight as a **directed edge** with time conditions.

---

### 🧠 Problem Breakdown

- You start at the `origin` airport at **time = 0**.
- You can take a flight from A to B only if:
  - You arrive at A before the **departure time** of the flight.
- Goal: Can you reach the `destination` airport?

---

### ✅ Approach

1. **Build an adjacency list**:
   - Map each airport to a list of possible outgoing flights.
   - Each flight has: destination, departure time, arrival time.
   
2. **BFS or Dijkstra-like traversal**:
   - Use a queue to simulate traversal: each entry is `(currentAirport, currentTime)`.
   - For each flight from `currentAirport`:
     - If `departureTime >= currentTime`, you can take the flight.
     - Enqueue `(nextAirport, arrivalTime)`.

3. Track visited states: `(airport, earliestTimeArrived)` to avoid cycles and redundant paths.

---

### 🧪 Example 1

Flights:
- NYC → LAX (0 → 4)
- LAX → SFO (5 → 7)

You arrive at NYC at `0`, take the first flight to LAX, arrive at `4`. Next flight from LAX departs at `5` (OK), arrives at `7`.

✅ Reach SFO → **True**

---

### ❌ Example 2

Flights:
- NYC → LAX (0 → 4)
- LAX → SFO (3 → 5)

You arrive at LAX at `4`, but the flight to SFO already left at `3`.

❌ Cannot reach → **False**

---

### 🚀 Golang Code

```go
package main

import (
	"fmt"
)

type Flight struct {
	from, to       string
	depart, arrive int
}

func canReachDestination(origin, destination string, flights []Flight) bool {
	adj := make(map[string][]Flight)
	for _, f := range flights {
		adj[f.from] = append(adj[f.from], f)
	}

	// Queue stores (currentAirport, arrivalTime)
	queue := []struct {
		airport string
		time    int
	}{{origin, 0}}

	// Visited map to avoid re-processing
	visited := make(map[string]int)

	for len(queue) > 0 {
		curr := queue[0]
		queue = queue[1:]

		// If reached destination
		if curr.airport == destination {
			return true
		}

		// Already visited this airport earlier at an earlier time
		if t, ok := visited[curr.airport]; ok && curr.time > t {
			continue
		}
		visited[curr.airport] = curr.time

		// Traverse valid flights
		for _, f := range adj[curr.airport] {
			if f.depart >= curr.time {
				queue = append(queue, struct {
					airport string
					time    int
				}{f.to, f.arrive})
			}
		}
	}

	return false
}

func main() {
	flights1 := []Flight{
		{"NYC", "LAX", 0, 4},
		{"LAX", "SFO", 5, 7},
	}
	fmt.Println(canReachDestination("NYC", "SFO", flights1)) // true

	flights2 := []Flight{
		{"NYC", "LAX", 0, 4},
		{"LAX", "SFO", 3, 5},
	}
	fmt.Println(canReachDestination("NYC", "SFO", flights2)) // false
}
```

---

### 🧠 Time Complexity

- Let `n` be number of flights.
- In the worst case, you may traverse all flights once: **O(n)**.
- Using `visited` avoids infinite loops or redundant checks.

---

Want a visual dry run on the first example or walk through a custom case?
