# Merge k Sorted Lists

## Metadata

- **Date Solved:** February 8, 2024 6:57 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/merge-k-sorted-lists/description/
- **Algorithm:** heap, two pointer
- **Type:** array, linked list

## Problem Statement

You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

## Constraints

- k == lists.length
- 0 <= k <= 104
- 0 <= lists[i].length <= 500
- -104 <= lists[i][j] <= 104
- lists[i] is sorted in ascending order.
- The sum of lists[i].length will not exceed 104.

## Solution

- **Status:** Solved On Own


### Brute Force

O(N * k)

```go
package main

func mergeKLists(lists []*ListNode) *ListNode {
	ans := &ListNode{}
	tmpAns := ans
	entriesLeft := true
	for entriesLeft {
		var minIdx int = -1
		for i := 0; i < len(lists); i++ {
			if lists[i] == nil {
				continue
			}
			if minIdx == -1 || lists[minIdx].Val > lists[i].Val {
				minIdx = i
			}
		}
		if minIdx == -1 {
			entriesLeft = false
		} else {
			tmpAns.Next = &ListNode{Val: lists[minIdx].Val, Next: nil}
			tmpAns = tmpAns.Next
			lists[minIdx] = lists[minIdx].Next
		}
	}
	return ans.Next
}
```

### Heap

O(N log k)

```go
package main

import "container/heap"

type ListPQ []*ListNode

func (q ListPQ) Len() int      { return len(q) }
func (q ListPQ) Swap(i, j int) { q[i], q[j] = q[j], q[i] }
func (q ListPQ) Less(i, j int) bool {
	return q[i].Val < q[j].Val
}
func (q *ListPQ) Push(x interface{}) {
	*q = append(*q, x.(*ListNode))
}

func (q *ListPQ) Pop() interface{} {
	tmp := *q
	n := len(tmp)
	x := tmp[n-1]
	*q = tmp[:n-1]
	return x
}

func mergeKLists(lists []*ListNode) *ListNode {
	ans := &ListNode{}
	tmpAns := ans

	q := &ListPQ{}
	heap.Init(q)

	for i := 0; i < len(lists); i++ {
		if lists[i] != nil {
			heap.Push(q, lists[i])
		}
	}
	for q.Len() > 0 {
		cur := heap.Pop(q).(*ListNode)
		tmpAns.Next = &ListNode{Val: cur.Val, Next: nil}
		tmpAns = tmpAns.Next
		if cur.Next != nil {
			heap.Push(q, cur.Next)
		}
	}
	return ans.Next
}
```
