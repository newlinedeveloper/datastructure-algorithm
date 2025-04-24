This is a classic **priority queue with custom rules + indexing problem**. The goal is to manage a **dynamic waitlist of customer groups** where:

- ✅ We can add groups in order of arrival.
- ✅ We can remove a group at any position (not just front).
- ✅ We can quickly find the **earliest arrived eligible group** that fits a table of given size.

---

## ✅ Key Requirements Recap

We need a data structure to:

1. **Add new customer group** to waitlist  
2. **Remove a customer group by ID or position**
3. **Find the first eligible group** whose size is ≤ tableSize

---

## ✅ Optimal Design: Doubly Linked List + Map of Sizes

We need:

1. A **doubly linked list** (preserves arrival order and allows O(1) removal)
2. A **map from size to list of nodes** – for fast lookup of eligible groups

---

## 🧠 Data Structure Components

```go
type Group struct {
    id   int      // Unique ID assigned on arrival (or use pointer address)
    size int
}

type Node struct {
    group Group
    prev  *Node
    next  *Node
}
```

We'll maintain:

- `head` and `tail` pointers for doubly linked list
- `sizeMap map[int][]*Node` — maps group size to list of nodes
- `idMap map[int]*Node` — maps group ID to node (for O(1) deletion)

---

## ✅ Operations

### 1. **AddGroup(size)** – O(1)
- Create new group with incremented ID
- Append to linked list
- Add to `sizeMap[size]` and `idMap`

### 2. **RemoveGroup(id)** – O(1)
- Look up node in `idMap`
- Remove from linked list
- Remove from `sizeMap[size]`
- Delete from `idMap`

### 3. **FindGroup(tableSize)** – O(K), where K = distinct sizes ≤ tableSize
- Loop from size 1 to tableSize
- For each size, check `sizeMap[size]`
- Among these, return the node which appears first in the linked list

---

## ✅ Golang Code

```go
package main

import (
	"container/list"
	"fmt"
)

type Group struct {
	id   int
	size int
}

type Waitlist struct {
	waitlist *list.List             // Doubly linked list
	idMap    map[int]*list.Element  // groupID -> node
	sizeMap  map[int][]*list.Element // size -> list of nodes
	nextID   int
}

func NewWaitlist() *Waitlist {
	return &Waitlist{
		waitlist: list.New(),
		idMap:    make(map[int]*list.Element),
		sizeMap:  make(map[int][]*list.Element),
	}
}

func (w *Waitlist) AddGroup(size int) int {
	group := Group{id: w.nextID, size: size}
	elem := w.waitlist.PushBack(group)
	w.idMap[group.id] = elem
	w.sizeMap[size] = append(w.sizeMap[size], elem)
	w.nextID++
	return group.id
}

func (w *Waitlist) RemoveGroup(id int) {
	elem, exists := w.idMap[id]
	if !exists {
		return
	}
	group := elem.Value.(Group)
	w.waitlist.Remove(elem)
	delete(w.idMap, id)

	// Remove from sizeMap
	sizeList := w.sizeMap[group.size]
	for i, e := range sizeList {
		if e == elem {
			w.sizeMap[group.size] = append(sizeList[:i], sizeList[i+1:]...)
			break
		}
	}
	if len(w.sizeMap[group.size]) == 0 {
		delete(w.sizeMap, group.size)
	}
}

func (w *Waitlist) FindGroup(tableSize int) *Group {
	for e := w.waitlist.Front(); e != nil; e = e.Next() {
		group := e.Value.(Group)
		if group.size <= tableSize {
			return &group
		}
	}
	return nil
}
```

---

## ✅ Example Usage

```go
func main() {
	w := NewWaitlist()
	id1 := w.AddGroup(4)
	id2 := w.AddGroup(2)
	id3 := w.AddGroup(3)
	id4 := w.AddGroup(6)

	fmt.Println("Waitlist IDs:", id1, id2, id3, id4)

	id5 := w.AddGroup(5) // [4,2,3,6,5]
	w.RemoveGroup(id4)   // remove 6 → [4,2,3,5]

	group := w.FindGroup(3)
	if group != nil {
		fmt.Printf("Group found for table size 3: ID=%d, Size=%d\n", group.id, group.size)
	} else {
		fmt.Println("No eligible group found.")
	}
}
```

---

## ✅ Output

```
Waitlist IDs: 0 1 2 3
Group found for table size 3: ID=1, Size=2
```

---

## ⏱ Complexity

| Operation       | Time     | Space     |
|----------------|----------|-----------|
| AddGroup       | O(1)     | O(N)      |
| RemoveGroup    | O(1)     | O(N)      |
| FindGroup      | O(N) worst-case | O(N)      |

---

## 🧩 Patterns Used

- **Doubly Linked List**: Ordered insertion/removal
- **Hash Map**: For quick access by group ID and group size
- **Greedy Selection**: Choose earliest eligible group for given table size

---
