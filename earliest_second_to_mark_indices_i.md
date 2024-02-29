# Earliest Second to Mark Indices I

## Metadata

- **Date Solved:** February 28, 2024 11:34 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/earliest-second-to-mark-indices-i/description/
- **Algorithm:** binary search, brute force
- **Type:** array, scenario

## Problem Statement

You are given two 1-indexed integer arrays, nums and, changeIndices, having lengths n and m, respectively.

Initially, all indices in nums are unmarked. Your task is to mark all indices in nums.

In each second, s, in order from 1 to m (inclusive), you can perform one of the following operations:

- Choose an index i in the range [1, n] and decrement nums[i] by 1.
- If nums[changeIndices[s]] is equal to 0, mark the index changeIndices[s].
- Do nothing.

Return an integer denoting the earliest second in the range [1, m] when all indices in nums can be marked by choosing operations optimally, or -1 if it is impossible.

## Constraints

- 1 <= n == nums.length <= 2000
- 0 <= nums[i] <= 109
- 1 <= m == changeIndices.length <= 2000
- 1 <= changeIndices[i] <= n

## Solution

- **Status:** Looked At Discuss

### Solution Notes

This problem was very tricky for a medium. I had to look at all the hints and still could not solve it. Discussion showed me the way.

In hindsight, coming up with a brute force solution may have helped, though I mostly only came up with it after learning the intuition.

The key here is to continuously keep the latest index encountered for a given num[I] in changeIndices. With this, we can maintain a count of decrements and once we reach an index j that equals the last encountered index for a given num, we decrement the count. If all nums are satisified we return

The optimization is to do binary search


### Brute Force

```go
package main

func earliestSecondToMarkIndices(nums []int, changeIndices []int) int {
	sumNums := 0
	for i := range nums {
		sumNums += nums[i]
	}
	if sumNums+len(nums) > len(changeIndices) {
		return -1
	}
	last := make(map[int]int)
Outer:
	for i := 0; i < len(changeIndices); i++ {
		for j := 0; j <= i; j++ {
			last[changeIndices[j]] = j
		}
		if len(last) != len(nums) {
			continue
		}
		count := 0
		for j := 0; j <= i; j++ {
			if j == last[changeIndices[j]] {
				if count < nums[changeIndices[j]-1] {
					continue Outer
				}
				count -= nums[changeIndices[j]-1]
			} else {
				count++
			}
		}
		return i + 1
	}
	return -1
}
```

### Binary Search

```go
package main
```
