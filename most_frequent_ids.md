# Most Frequent IDs

## Metadata

- **Date Solved:** April 8, 2024 12:01 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/most-frequent-ids/description/
- **Algorithm:** hashmap, heap
- **Type:** array

## Problem Statement

The problem involves tracking the frequency of IDs in a collection that changes over time. You have two integer arrays, nums and freq, of equal length n. Each element in nums represents an ID, and the corresponding element in freq indicates how many times that ID should be added to or removed from the collection at each step.

- Addition of IDs: If freq[i] is positive, it means freq[i] IDs with the value nums[i] are added to the collection at step i.
- Removal of IDs: If freq[i] is negative, it means -freq[i] IDs with the value nums[i] are removed from the collection at step i.

Return an array ans of length n, where ans[i] represents the count of the most frequent ID in the collection after the ith step. If the collection is empty at any step, ans[i] should be 0 for that step.

## Constraints


- 1 <= nums.length == freq.length <= 105
- 1 <= nums[i] <= 105
- -105 <= freq[i] <= 105
- freq[i] != 0
- The input is generated such that the occurrences of an ID will not be negative in any step.

## Solution

- **Status:** Solved On Own


```go
package main

import "container/heap"

var IDFreq map[int]*ID

func mostFrequentIDs(nums []int, freq []int) []int64 {
	IDFreq = make(map[int]*ID)
	idHeap := make(MaxHeapPQ, 0, len(nums))
	for i := range nums {
		if _, ok := IDFreq[nums[i]]; !ok {
			id := ID{}
			IDFreq[nums[i]] = &id
			idHeap.Push(&id)
		}
	}
	heap.Init(&idHeap)
	ans := make([]int64, len(nums))
	for i, f := range freq {
		IDFreq[nums[i]].freq += int64(f)
		heap.Fix(&idHeap, IDFreq[nums[i]].index)
		ans[i] = idHeap.Peek().(*ID).freq
	}
	return ans
}

type ID struct {
	index int
	freq  int64
}

type MaxHeapPQ []*ID

func (h MaxHeapPQ) Len() int { return len(h) }
func (h MaxHeapPQ) Less(i, j int) bool {
	return h[i].freq > h[j].freq
}
func (h MaxHeapPQ) Swap(i, j int) {
	h[i], h[j] = h[j], h[i]
	h[i].index = i
	h[j].index = j
}

func (h *MaxHeapPQ) Push(x any) {
	x.(*ID).index = h.Len()
	*h = append(*h, x.(*ID))
}

func (h *MaxHeapPQ) Pop() any {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}
func (h MaxHeapPQ) Peek() any { return h[0] }
```
