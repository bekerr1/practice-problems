# Sliding Window Maximum

## Metadata

- **Date Solved:** July 11, 2022 8:17 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/sliding-window-maximum/
- **Algorithm:** heap
- **Type:** dequeue

## Problem Statement

You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.
Return the max sliding window.

## Problem


### Heap - O(nlogn) Accepted

```go
package main

import "container/heap"

type ScoreHeap [][2]int

func (h ScoreHeap) Len() int           { return len(h) }
func (h ScoreHeap) Less(i, j int) bool { return h[i][0] > h[j][0] }
func (h ScoreHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *ScoreHeap) Push(x interface{}) {
	*h = append(*h, x.([2]int))
}

func (h ScoreHeap) Peek() interface{} {
	return h[0]
}

func (h *ScoreHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func maxSlidingWindow(nums []int, k int) []int {

	ret := make([]int, 0, len(nums)/k)
	pq := &ScoreHeap{}
	heap.Init(pq)

	var curMax [2]int
	for i := 0; i < len(nums); i++ {

		heap.Push(pq, [2]int{nums[i], i})

		curMax = pq.Peek().([2]int)
		for curMax[1] <= i-k {
			heap.Pop(pq)
			curMax = pq.Peek().([2]int)
		}

		if i >= k-1 {
			ret = append(ret, curMax[0])
		}
	}
	return ret
}
```

### Dequeue - O(n) Accepted

```go
package main

import "container/list"

func maxSlidingWindow(nums []int, k int) []int {
	ret := make([]int, 0, len(nums)-k+1)
	dequeue := list.New()

	for i := 0; i < len(nums); i++ {

		for front := dequeue.Front(); front != nil &&
			queueValueOutOfRange(front, i, k); front = dequeue.Front() {
			dequeue.Remove(front)
		}

		for back := dequeue.Back(); back != nil &&
			queueValueLessThanCurrent(nums, back, i); back = dequeue.Back() {
			dequeue.Remove(back)
		}

		dequeue.PushBack(i)
		if i >= k-1 {
			front := dequeue.Front().Value.(int)
			ret = append(ret, nums[front])
		}
	}
	return ret
}

func queueValueLessThanCurrent(
	nums []int,
	queueValue *list.Element,
	current int,
) bool {
	v := queueValue.Value.(int)
	return nums[v] < nums[current]
}

func queueValueOutOfRange(
	queueValue *list.Element,
	curIdx, k int,
) bool {
	v := queueValue.Value.(int)
	return v <= curIdx-k
}
```
