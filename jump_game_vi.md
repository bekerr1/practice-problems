# Jump Game VI

## Metadata

- **Date Solved:** July 10, 2022 4:54 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/jump-game-vi/
- **Algorithm:** dynamic programming
- **Type:** heap

## Problem Statement

You are given a 0-indexed integer array nums and an integer k.
You are initially standing at index 0. In one move, you can jump at most k steps forward without going outside the boundaries of the array. That is, you can jump from index i to any index in the range [i + 1, min(n - 1, i + k)] inclusive.
You want to reach the last index of the array (index n - 1). Your score is the sum of all nums[j] for each index j you visited in the array.
Return the maximum score you can get.

## Problem


### Brute Force - DP O(nk) (Time Limit Exceeded)

```go
package main

import "math"

func maxResult(nums []int, k int) int {
	dp := make([]int, len(nums))
	for i := range dp {
		dp[i] = math.MinInt
	}
	dp[0] = nums[0]
	for i := 1; i < len(nums); i++ {
		maxVal := math.MinInt
		for j := 1; j <= k; j++ {
			if i-j < 0 {
				continue
			}
			maxVal = max(maxVal, dp[i-j])
		}
		dp[i] = maxVal + nums[i]
	}
	return dp[len(nums)-1]
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Optimized - Heap - O(nlogn)

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

func maxResult(nums []int, k int) int {
	pq := &ScoreHeap{[2]int{nums[0], 0}}
	heap.Init(pq)

	curMaxVal := nums[0]
	for i := 1; i < len(nums); i++ {
		curMax := pq.Peek().([2]int)
		maxVal, idx := curMax[0], curMax[1]

		for idx < i-k {
			heap.Pop(pq)
			curMax := pq.Peek().([2]int)
			maxVal, idx = curMax[0], curMax[1]
		}

		curMaxVal = maxVal + nums[i]
		heap.Push(pq, [2]int{curMaxVal, i})
	}
	return curMaxVal
}
```

## Solution Statement


Here, the brute force method, even though it uses DP, was not enough as we have to traverse all ‘k’ possible maximums for every entry in the array. The optimized solution is to use a heap and maintain the heap entries to ensure only the last ‘k’ max values are present. Then the problem becomes…

“For the current max value in the heap, given all entries in the heap are within i - k, add that value to the current number. Store this number in the heap. By the end of the loop, the max value of all entries will result.”
