### Linked List

#### Problem 1 -  Easy : Reverse Linked List

```
type ListNode struct {
    Val int
    Next *ListNode
}

func reverseList(head *ListNode) *ListNode {
    var prev *ListNode = nil
    curr := head
    for curr != nil {
        next := curr.Next
        curr.Next = prev
        prev = curr
        curr = next
    }
    return prev
}


```

This function takes the head of a singly linked list as input and returns the head of the reversed linked list. It iterates through the list, reversing the direction of each link as it goes. The prev variable keeps track of the previous node in the reversed list, and the curr variable keeps track of the current node being reversed. The next variable temporarily stores a reference to the next node in the original list so that we can move to it in the next iteration. Finally, we return the prev variable as the new head of the reversed list.


```
func main() {
    // create linked list: 1 -> 2 -> 3 -> 4 -> 5
    head := &ListNode{Val: 1, Next: &ListNode{Val: 2, Next: &ListNode{Val: 3, Next: &ListNode{Val: 4, Next: &ListNode{Val: 5, Next: nil}}}}}

    // reverse linked list
    head = reverseList(head)

    // print reversed list: 5 -> 4 -> 3 -> 2 -> 1
    for curr := head; curr != nil; curr = curr.Next {
        fmt.Println(curr.Val)
    }
}


```

#### Problem 2 -  Easy : Merge Two Sorted Lists

```
type ListNode struct {
    Val int
    Next *ListNode
}

func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := &ListNode{Val: 0, Next: nil}
    curr := dummy
    for l1 != nil && l2 != nil {
        if l1.Val < l2.Val {
            curr.Next = l1
            l1 = l1.Next
        } else {
            curr.Next = l2
            l2 = l2.Next
        }
        curr = curr.Next
    }
    if l1 != nil {
        curr.Next = l1
    } else {
        curr.Next = l2
    }
    return dummy.Next
}


```


This function takes the heads of two sorted linked lists, l1 and l2, as input and returns the head of the merged sorted linked list. It iterates through the lists, comparing the values of the current nodes in l1 and l2, and adding the smaller value to the new list. The dummy node is used as a placeholder for the head of the new list, so that we can return dummy.Next at the end.

```
func main() {
    // create linked list 1: 1 -> 2 -> 4
    l1 := &ListNode{Val: 1, Next: &ListNode{Val: 2, Next: &ListNode{Val: 4, Next: nil}}}

    // create linked list 2: 1 -> 3 -> 4
    l2 := &ListNode{Val: 1, Next: &ListNode{Val: 3, Next: &ListNode{Val: 4, Next: nil}}}

    // merge two linked lists
    merged := mergeTwoLists(l1, l2)

    // print merged list: 1 -> 1 -> 2 -> 3 -> 4 -> 4
    for curr := merged; curr != nil; curr = curr.Next {
        fmt.Println(curr.Val)
    }
}



```


#### Problem 3 -  Medium : Reorder List

```
type ListNode struct {
    Val int
    Next *ListNode
}

func reorderList(head *ListNode) {
    if head == nil || head.Next == nil {
        return
    }
    // step 1: find the middle of the list
    slow, fast := head, head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }
    // step 2: reverse the second half of the list
    var prev *ListNode = nil
    curr := slow.Next
    slow.Next = nil
    for curr != nil {
        next := curr.Next
        curr.Next = prev
        prev = curr
        curr = next
    }
    // step 3: merge the first and second halves of the list
    l1, l2 := head, prev
    for l2 != nil {
        next1 := l1.Next
        next2 := l2.Next
        l1.Next = l2
        l2.Next = next1
        l1 = next1
        l2 = next2
    }
}


```
This function takes the head of a linked list as input and reorders it in-place so that the first half of the list is interleaved with the second half of the list in reverse order.

The algorithm consists of three steps:

- Find the middle of the list using the slow and fast pointer technique.
- Reverse the second half of the list from the middle node.
- Merge the first and second halves of the list by interweaving the nodes.

```
func main() {
    // create linked list: 1 -> 2 -> 3 -> 4 -> 5
    head := &ListNode{Val: 1, Next: &ListNode{Val: 2, Next: &ListNode{Val: 3, Next: &ListNode{Val: 4, Next: &ListNode{Val: 5, Next: nil}}}}}

    // reorder linked list: 1 -> 5 -> 2 -> 4 -> 3
    reorderList(head)

    // print reordered list
    for curr := head; curr != nil; curr = curr.Next {
        fmt.Println(curr.Val)
    }
}


```


#### Problem 4 -  Medium : Remove Nth Node From End of List

Certainly! Here's an example implementation of removing the nth node from the end of a linked list using Go:

```
type ListNode struct {
    Val int
    Next *ListNode
}

func removeNthFromEnd(head *ListNode, n int) *ListNode {
    if head == nil {
        return nil
    }
    // step 1: find the length of the list
    length := 0
    curr := head
    for curr != nil {
        length++
        curr = curr.Next
    }
    // step 2: find the node before the node to be removed
    dummy := &ListNode{Val: 0, Next: head}
    curr = dummy
    for i := 0; i < length-n; i++ {
        curr = curr.Next
    }
    // step 3: remove the node
    curr.Next = curr.Next.Next
    return dummy.Next
}


```

This function takes the head of a linked list and an integer n as input, and removes the nth node from the end of the list.

The algorithm consists of three steps:

- Find the length of the list.
- Find the node before the node to be removed.
- Remove the node.

Here's an example usage:

```
func main() {
    // create linked list: 1 -> 2 -> 3 -> 4 -> 5
    head := &ListNode{Val: 1, Next: &ListNode{Val: 2, Next: &ListNode{Val: 3, Next: &ListNode{Val: 4, Next: &ListNode{Val: 5, Next: nil}}}}}

    // remove 2nd node from the end: 1 -> 2 -> 3 -> 5
    newHead := removeNthFromEnd(head, 2)

    // print new list
    for curr := newHead; curr != nil; curr = curr.Next {
        fmt.Println(curr.Val)
    }
}


```



#### Problem 5 -  Medium : Copy List With Random Pointer

```
type Node struct {
    Val int
    Next *Node
    Random *Node
}

func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }
    // step 1: duplicate each node and insert it after the original node
    curr := head
    for curr != nil {
        next := curr.Next
        curr.Next = &Node{Val: curr.Val}
        curr.Next.Next = next
        curr = next
    }
    // step 2: assign the random pointers for the duplicated nodes
    curr = head
    for curr != nil {
        if curr.Random != nil {
            curr.Next.Random = curr.Random.Next
        }
        curr = curr.Next.Next
    }
    // step 3: separate the duplicated nodes from the original nodes
    dummy := &Node{Val: 0, Next: nil}
    curr = head
    copyCurr := dummy
    for curr != nil {
        next := curr.Next.Next
        copyCurr.Next = curr.Next
        copyCurr = copyCurr.Next
        curr.Next = next
        curr = next
    }
    return dummy.Next
}


```
This function takes the head of a linked list with a random pointer as input, and returns a deep copy of the list.

The algorithm consists of three steps:

- Duplicate each node and insert it after the original node.
- Assign the random pointers for the duplicated nodes.
- Separate the duplicated nodes from the original nodes.

```
func main() {
    // create linked list with random pointers: 1 -> 2 -> 3 -> 4 -> 5
    head := &Node{Val: 1, Next: &Node{Val: 2, Next: &Node{Val: 3, Next: &Node{Val: 4, Next: &Node{Val: 5, Next: nil}}}}}
    head.Random = head.Next.Next
    head.Next.Random = head.Next.Next.Next

    // copy linked list
    copyHead := copyRandomList(head)

    // print original list
    for curr := head; curr != nil; curr = curr.Next {
        fmt.Println(curr.Val, curr.Random)
    }

    // print copied list
    for curr := copyHead; curr != nil; curr = curr.Next {
        fmt.Println(curr.Val, curr.Random)
    }
}


```


#### Problem 6 -  Medium : Add Two Numbers

```
type ListNode struct {
    Val int
    Next *ListNode
}

func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := &ListNode{Val: 0, Next: nil}
    curr := dummy
    carry := 0
    for l1 != nil || l2 != nil {
        val1, val2 := 0, 0
        if l1 != nil {
            val1 = l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            val2 = l2.Val
            l2 = l2.Next
        }
        sum := val1 + val2 + carry
        carry = sum / 10
        curr.Next = &ListNode{Val: sum % 10, Next: nil}
        curr = curr.Next
    }
    if carry > 0 {
        curr.Next = &ListNode{Val: carry, Next: nil}
    }
    return dummy.Next
}


```

This function takes two linked lists representing non-negative integers as input, and returns a new linked list representing the sum of the input lists.

The algorithm iterates through both lists simultaneously, adding the corresponding nodes together along with any carry from the previous addition. A new node is created to store the sum of the two nodes, and the carry is updated accordingly. Finally, if there is a carry remaining after the last addition, a new node is created to store the carry.

Here's an example usage:

```
func main() {
    // create linked lists: 2 -> 4 -> 3 and 5 -> 6 -> 4
    l1 := &ListNode{Val: 2, Next: &ListNode{Val: 4, Next: &ListNode{Val: 3, Next: nil}}}
    l2 := &ListNode{Val: 5, Next: &ListNode{Val: 6, Next: &ListNode{Val: 4, Next: nil}}}

    // add two linked lists: 7 -> 0 -> 8
    sum := addTwoNumbers(l1, l2)

    // print sum
    for curr := sum; curr != nil; curr = curr.Next {
        fmt.Println(curr.Val)
    }
}

```




#### Problem 7 -  Easy : Linked List Cycle

```
type ListNode struct {
    Val int
    Next *ListNode
}

func hasCycle(head *ListNode) bool {
    if head == nil {
        return false
    }
    slow := head
    fast := head.Next
    for slow != nil && fast != nil && fast.Next != nil {
        if slow == fast {
            return true
        }
        slow = slow.Next
        fast = fast.Next.Next
    }
    return false
}


```

This function takes the head of a linked list as input, and returns a boolean indicating whether the list contains a cycle.

The algorithm uses two pointers, slow and fast, to traverse the list. The slow pointer moves one node at a time, while the fast pointer moves two nodes at a time. If there is a cycle in the list, the slow and fast pointers will eventually meet.

Here's an example usage:


```
func main() {
    // create linked list with cycle: 1 -> 2 -> 3 -> 4 -> 5 -> 2
    head := &ListNode{Val: 1, Next: &ListNode{Val: 2, Next: &ListNode{Val: 3, Next: &ListNode{Val: 4, Next: &ListNode{Val: 5, Next: nil}}}}}
    head.Next.Next.Next.Next.Next = head.Next

    // check if linked list has a cycle
    hasCycle := hasCycle(head)
    fmt.Println(hasCycle)
}


```



#### Problem 8 -  Medium : Find The Duplicate Number


Finding the duplicate number in a linked list can be done using Floyd's cycle detection algorithm, which is also used to detect cycles in a linked list. Here's an example implementation in Go:

```
type ListNode struct {
    Val int
    Next *ListNode
}

func findDuplicate(head *ListNode) int {
    slow := head
    fast := head

    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
        if slow == fast {
            break
        }
    }

    if fast == nil || fast.Next == nil {
        return -1 // no cycle found
    }

    slow = head
    for slow != fast {
        slow = slow.Next
        fast = fast.Next
    }

    return slow.Val // return the value of the duplicate node
}


```

This function takes the head of a linked list as input, and returns the value of the duplicate node in the list.

The algorithm uses two pointers, slow and fast, to detect a cycle in the list. If a cycle is detected, the slow pointer is moved back to the head of the list, and both slow and fast pointers are moved one node at a time until they meet again. The node they meet at is the duplicate node.

Here's an example usage:


```
func main() {
    // create linked list with duplicate: 1 -> 3 -> 4 -> 2 -> 2
    head := &ListNode{Val: 1, Next: &ListNode{Val: 3, Next: &ListNode{Val: 4, Next: &ListNode{Val: 2, Next: &ListNode{Val: 2, Next: nil}}}}}

    // find the duplicate number in the linked list
    duplicate := findDuplicate(head)
    fmt.Println(duplicate)
}


```


#### Problem 9 -  Medium : LRU Cache


```
type Node struct {
    Key, Val int
    Prev, Next *Node
}

type LRUCache struct {
    Capacity int
    Map map[int]*Node
    Head, Tail *Node
}

func Constructor(capacity int) LRUCache {
    return LRUCache{
        Capacity: capacity,
        Map: make(map[int]*Node),
        Head: &Node{},
        Tail: &Node{},
    }
}

func (this *LRUCache) Get(key int) int {
    if node, ok := this.Map[key]; ok {
        this.moveToFront(node)
        return node.Val
    }
    return -1
}

func (this *LRUCache) Put(key int, value int) {
    if node, ok := this.Map[key]; ok {
        node.Val = value
        this.moveToFront(node)
        return
    }
    if len(this.Map) == this.Capacity {
        delete(this.Map, this.Tail.Key)
        this.removeFromTail()
    }
    node := &Node{Key: key, Val: value}
    this.Map[key] = node
    this.addToFront(node)
}

func (this *LRUCache) moveToFront(node *Node) {
    this.removeFromList(node)
    this.addToFront(node)
}

func (this *LRUCache) removeFromTail() {
    this.removeFromList(this.Tail.Prev)
}

func (this *LRUCache) removeFromList(node *Node) {
    node.Prev.Next = node.Next
    node.Next.Prev = node.Prev
}

func (this *LRUCache) addToFront(node *Node) {
    node.Next = this.Head.Next
    node.Prev = this.Head
    this.Head.Next.Prev = node
    this.Head.Next = node
}


```
This implementation of LRU cache uses a doubly linked list to keep track of the order of nodes, with the most recently used node at the front of the list and the least recently used node at the back of the list. The hash map is used to quickly lookup nodes by their key.

The Get method returns the value of the node with the given key, and moves the node to the front of the list.

The Put method adds a new node with the given key and value to the cache, or updates the value of an existing node. If the cache is at capacity, the least recently used node is removed from the cache.

The moveToFront, removeFromTail, removeFromList, and addToFront methods are used to manipulate the doubly linked list.

Here's an example usage:

```
func main() {
    // create LRU cache with capacity 2
    cache := Constructor(2)

    // add some key-value pairs to the cache
    cache.Put(1, 1)
    cache.Put(2, 2)

    // retrieve some values from the cache
    fmt.Println(cache.Get(1)) // should print 1
    fmt.Println(cache.Get(2)) // should print 2

    // add another key-value pair to the cache
    cache.Put(3, 3)

    // the least recently used key (1) should have been removed from the cache
    fmt.Println(cache.Get(1)) // should print -1
}



```

#### Problem 10 -  Hard : Merge K Sorted Lists


```
type ListNode struct {
    Val  int
    Next *ListNode
}

func mergeKLists(lists []*ListNode) *ListNode {
    if len(lists) == 0 {
        return nil
    }
    if len(lists) == 1 {
        return lists[0]
    }
    mid := len(lists) / 2
    left := mergeKLists(lists[:mid])
    right := mergeKLists(lists[mid:])
    return mergeTwoLists(left, right)
}

func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := &ListNode{}
    curr := dummy
    for l1 != nil && l2 != nil {
        if l1.Val < l2.Val {
            curr.Next = l1
            l1 = l1.Next
        } else {
            curr.Next = l2
            l2 = l2.Next
        }
        curr = curr.Next
    }
    if l1 != nil {
        curr.Next = l1
    }
    if l2 != nil {
        curr.Next = l2
    }
    return dummy.Next
}


```

The ListNode struct is the same as the one used in previous examples.

The mergeKLists function recursively divides the list of lists in two halves until there is only one or none left, and then merges them using mergeTwoLists function.

The mergeTwoLists function merges two sorted linked lists into a single sorted list.

Here's an example usage:

```
func main() {
    // create three sorted linked lists
    list1 := &ListNode{1, &ListNode{4, &ListNode{5, nil}}}
    list2 := &ListNode{1, &ListNode{3, &ListNode{4, nil}}}
    list3 := &ListNode{2, &ListNode{6, nil}}

    // merge the three lists into a single sorted list
    merged := mergeKLists([]*ListNode{list1, list2, list3})

    // print the merged list
    for merged != nil {
        fmt.Println(merged.Val)
        merged = merged.Next
    }
}


```


#### Problem 11 -  Hard : Reverse Nodes In K Group

```
type ListNode struct {
    Val  int
    Next *ListNode
}

func reverseKGroup(head *ListNode, k int) *ListNode {
    if head == nil {
        return nil
    }
    var prev, curr, next *ListNode = nil, head, nil
    var count int = 0
    var newHead *ListNode = nil
    for curr != nil && count < k {
        next = curr.Next
        curr.Next = prev
        prev = curr
        curr = next
        count++
    }
    if count == k {
        newHead = prev
        head.Next = reverseKGroup(curr, k)
    } else {
        newHead = reverseList(prev)
    }
    return newHead
}

func reverseList(head *ListNode) *ListNode {
    var prev, curr, next *ListNode = nil, head, nil
    for curr != nil {
        next = curr.Next
        curr.Next = prev
        prev = curr
        curr = next
    }
    return prev
}


```

The ListNode struct is the same as the one used in previous examples.

The reverseKGroup function takes the head of a linked list and an integer k, and reverses the nodes in the list in groups of k. If there are less than k nodes left at the end of the list, those nodes are left as-is.

The function uses two helper functions: reverseList which reverses a linked list, and reverseKGroup which recursively reverses the nodes in groups of k.

Here's an example usage:


```
func main() {
    // create a linked list
    head := &ListNode{1, &ListNode{2, &ListNode{3, &ListNode{4, &ListNode{5, nil}}}}}

    // reverse the nodes in groups of 2
    reversed := reverseKGroup(head, 2)

    // print the reversed list
    for reversed != nil {
        fmt.Println(reversed.Val)
        reversed = reversed.Next
    }
}


```

