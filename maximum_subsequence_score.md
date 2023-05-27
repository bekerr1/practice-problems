# Maximum Subsequence Score

## Metadata

- **Date Solved:** May 27, 2023 3:03 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-subsequence-score/description/
- **Algorithm:** heap, sort
- **Type:** array, greedy

## Problem Statement

You are given two 0-indexed integer arrays nums1 and nums2 of equal length n and a positive integer k. You must choose a subsequence of indices from nums1 of length k.
For chosen indices i0, i1, ..., ik - 1, your score is defined as:
- The sum of the selected elements from nums1 multiplied with the minimum of the selected elements from nums2.
- It can defined simply as: (nums1[i0] + nums1[i1] +...+ nums1[ik - 1]) * min(nums2[i0] , nums2[i1], ... ,nums2[ik - 1]).
Return the maximum possible score.
A subsequence of indices of an array is a set that can be derived from the set {0, 1, ..., n-1} by deleting some or no elements.

## Constraints

- n == nums1.length == nums2.length
- 1 <= n <= 105
- 0 <= nums1[i], nums2[j] <= 105
- 1 <= k <= n

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I got too caught up in trying to come up with a solution that handled every subsequence of pairs. This would be some dynamic programming solution, but the constraints would probably not allow it. O(n^2)

Instead, optimal solution was to sort the pairs descending by nums2 and keep a min heap of k nums1 entries. This way, we would always pop out the least nums1 entry to maximize the sum while multiplying by the min of nums2, since we sorted in decending order.


```go
package main

import (
	"container/heap"
	"sort"
)

// [2]int{nums2[i], nums1[i]}
type IntHeap []int

func (h IntHeap) Len() int            { return len(h) }
func (h IntHeap) Less(i, j int) bool  { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *IntHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func maxScore(nums1 []int, nums2 []int, k int) int64 {

	pairs := [][2]int{}
	for i := 0; i < len(nums1); i++ {
		pairs = append(pairs, [2]int{nums2[i], nums1[i]})
	}

	sort.Slice(pairs, func(i, j int) bool {
		return pairs[i][0] > pairs[j][0]
	})

	sum := 0
	pq := IntHeap{}
	for i := 0; i < k; i++ {
		sum += pairs[i][1]
		pq = append(pq, pairs[i][1])
	}
	heap.Init(&pq)

	var ret int64 = int64(sum * pairs[k-1][0])
	for i := k; i < len(pairs); i++ {
		least := heap.Pop(&pq).(int)
		sum -= least
		next := pairs[i][1]
		heap.Push(&pq, next)
		sum += next

		ret = max(ret, int64(sum*pairs[i][0]))
	}
	return ret
}

func max(x, y int64) int64 {
	if x > y {
		return x
	}
	return y
}
```
