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


#### Problem 6 -  Medium : Add Two Numbers


#### Problem 7 -  Easy : Linked List Cycle



#### Problem 8 -  Medium : Find The Duplicate Number


#### Problem 9 -  Medium : LRU Cache


#### Problem 10 -  Hard : Merge K Sorted Lists


#### Problem 11 -  Hard : Reverse Nodes In K Group
