### Heap / Priority Queue

#### Find Median From Data Stream

```
package main

import (
	"container/heap"
	"fmt"
)

type MaxHeap []int

func (h MaxHeap) Len() int            { return len(h) }
func (h MaxHeap) Less(i, j int) bool  { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MaxHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[:n-1]
	return x
}

type MinHeap []int

func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[:n-1]
	return x
}

type MedianFinder struct {
	maxHeap *MaxHeap
	minHeap *MinHeap
}

func Constructor() MedianFinder {
	maxHeap := &MaxHeap{}
	minHeap := &MinHeap{}
	heap.Init(maxHeap)
	heap.Init(minHeap)
	return MedianFinder{maxHeap, minHeap}
}

func (mf *MedianFinder) AddNum(num int) {
	if mf.maxHeap.Len() == 0 || num <= (*mf.maxHeap)[0] {
		heap.Push(mf.maxHeap, num)
	} else {
		heap.Push(mf.minHeap, num)
	}

	if mf.maxHeap.Len()-mf.minHeap.Len() > 1 {
		heap.Push(mf.minHeap, heap.Pop(mf.maxHeap))
	} else if mf.minHeap.Len()-mf.maxHeap.Len() > 0 {
		heap.Push(mf.maxHeap, heap.Pop(mf.minHeap))
	}
}

func (mf *MedianFinder) FindMedian() float64 {
	if mf.maxHeap.Len() > mf.minHeap.Len() {
		return float64((*mf.maxHeap)[0])
	} else {
		return (float64((*mf.maxHeap)[0]) + float64((*mf.minHeap)[0])) / 2
	}
}

func main() {
	mf := Constructor()

	mf.AddNum(1)
	fmt.Println(mf.FindMedian()) // Output: 1

	mf.AddNum(2)
	fmt.Println(mf.FindMedian()) // Output: 1.5

	mf.AddNum(3)
	fmt.Println(mf.FindMedian()) // Output: 2
}


```
In this code, we define two types: MaxHeap and MinHeap, which are custom implementations of the max heap and min heap, respectively. These types satisfy the heap interface from the container/heap package.

The MedianFinder struct holds two heaps: maxHeap to store the lower half of the data stream, and minHeap to store the upper half of the data stream.

The Constructor function initializes the MedianFinder by creating empty max and min heaps.

The AddNum method adds a number to the appropriate heap based on its value. It maintains the balance between the two heaps by moving elements between them if necessary.

The FindMedian method calculates and returns the median of the data stream. If the number of elements is odd, the median is the root of the max heap. If the number of elements is even, the median is the average of the roots of the max and min heaps.

In the main function, we create a new MedianFinder, add some numbers (1, 2, 3) to the data stream, and calculate the median after each addition to demonstrate the functionality of the data structure.

