# Find Score of an Array After Marking All Elements

## Metadata

- **Date Solved:** March 25, 2023 1:17 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-score-of-an-array-after-marking-all-elements/description/
- **Algorithm:** heap
- **Type:** array

## Problem Statement

You are given an array nums consisting of positive integers.
Starting with score = 0, apply the following algorithm:
- Choose the smallest integer of the array that is not marked. If there is a tie, choose the one with the smallest index.
- Add the value of the chosen integer to score.
- Mark the chosen element and its two adjacent elements if they exist.
- Repeat until all the array elements are marked.
Return the score you get after applying the above algorithm.

## Constraints

- 1 <= nums.length <= 105
- 1 <= nums[i] <= 106

## Solution

- **Status:** Solved On Own

### Solution Notes

Well defined trade off between space and time. If we wanted less time had to implement a data structure to hold len(nums) extra entries. If we did not want to do that, more time would need to be alloted.

It was good to use the nums array to repurpose marking adjacent elements rather than creating another data structure to do so.


```go
package main

import "container/heap"

var n []int

func findScore(nums []int) int64 {
	n = nums
	h := make(Heap, len(nums))
	for i := range nums {
		h[i] = i
	}
	var score int64
	heap.Init(&h)
	for h.Len() > 0 {
		cur := heap.Pop(&h).(int)
		score += int64(nums[cur])
		if nums[cur] > 0 && cur-1 >= 0 {
			nums[cur-1] = 0
		}
		if nums[cur] > 0 && cur+1 < len(nums) {
			nums[cur+1] = 0
		}
	}
	return score
}

// idx is [0] and value is [1]
type Heap []int

func (h Heap) Len() int { return len(h) }
func (h Heap) Less(i, j int) bool {
	if n[h[i]] == n[h[j]] {
		return h[i] < h[j]
	}
	return n[h[i]] < n[h[j]]
}
func (h Heap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }

func (h *Heap) Push(x interface{}) {
	// Push and Pop use pointer receivers because they modify the slice's length,
	// not just its contents.
	*h = append(*h, x.(int))
}

func (h *Heap) Pop() interface{} {
	tmp := *h
	n := len(tmp)
	val := tmp[n-1]
	*h = tmp[0 : n-1]
	return val
}
```
