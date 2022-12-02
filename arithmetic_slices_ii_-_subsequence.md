# Arithmetic Slices II - Subsequence

## Metadata

- **Date Solved:** December 2, 2022 5:19 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/arithmetic-slices-ii-subsequence/
- **Algorithm:** brute force, dynamic programming
- **Type:** array

## Problem Statement

Given an integer array nums, return the number of all the arithmetic subsequences of nums.
A sequence of numbers is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.
- For example, [1, 3, 5, 7, 9], [7, 7, 7, 7], and [3, -1, -5, -9] are arithmetic sequences.
- For example, [1, 1, 2, 5, 7] is not an arithmetic sequence.
A subsequence of an array is a sequence that can be formed by removing some elements (possibly none) of the array.
- For example, [2,5,10] is a subsequence of [1,2,1,2,4,1,5,10].
The test cases are generated so that the answer fits in 32-bit integer.

## Constraints

- 1  <= nums.length <= 1000
- -231 <= nums[i] <= 231 - 1

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Was able to come up with a solution via brute force, recomputing some cases. I was only doing a single for loop and trying to compute from the start 


### Brute Force (TLE)

```go
package main

import "math"

var sum int

func numberOfArithmeticSlices(nums []int) int {
	if len(nums) < 3 {
		return 0
	}
	sum = 0
	for i := 0; i < len(nums)-2; i++ {
		numberOfArithmeticSlicesUtil(nums, i, 1, math.MinInt)
	}
	return sum
}

func numberOfArithmeticSlicesUtil(nums []int, start, curLen, curDiff int) {
	if curLen >= 3 {
		sum += 1
	}
	for i := start + 1; i < len(nums); i++ {
		nextDiff := nums[start] - nums[i]
		if curDiff == math.MinInt || nextDiff == curDiff {
			numberOfArithmeticSlicesUtil(nums, i, curLen+1, nextDiff)
		}
	}
	return
}
```

### DP (memoization)

```go
package main

var memo map[[2]int]int
var sum int

func numberOfArithmeticSlices(nums []int) int {
	if len(nums) < 3 {
		return 0
	}
	memo = make(map[[2]int]int)
	sum = 0
	for i := 0; i < len(nums); i++ {
		for j := i + 1; j < len(nums); j++ {
			sum += numberOfArithmeticSlicesUtil(nums, j+1, nums[j], nums[j]-nums[i])
		}
	}
	return sum
}

func numberOfArithmeticSlicesUtil(nums []int, curIdx, curNum, curDiff int) int {

	if curIdx >= len(nums) {
		return 0
	}

	if c, exists := memo[[2]int{curIdx, curDiff}]; exists {
		return c
	}

	total := 0
	for i := curIdx; i < len(nums); i++ {
		if nums[i]-curNum == curDiff {
			total += 1 + numberOfArithmeticSlicesUtil(nums, i+1, nums[i], curDiff)
		}
	}
	memo[[2]int{curIdx, curDiff}] = total
	return total
}
```
