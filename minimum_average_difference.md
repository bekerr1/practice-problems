# Minimum Average Difference

## Metadata

- **Date Solved:** December 8, 2022 11:19 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-average-difference/
- **Algorithm:** prefixsum
- **Type:** array

## Problem Statement

You are given a 0-indexed integer array nums of length n.
The average difference of the index i is the absolute difference between the average of the first i + 1 elements of nums and the average of the last n - i - 1 elements. Both averages should be rounded down to the nearest integer.
Return the index with the minimum average difference. If there are multiple such indices, return the smallest one.
Note:
- The absolute difference of two numbers is the absolute value of their difference.
- The average of n elements is the sum of the n elements divided (integer division) by n.
- The average of 0 elements is considered to be 0.

## Constraints

- 1 <= nums.length <= 105
- 0 <= nums[i] <= 105

## Solution

- **Status:** Solved On Own


```go
package main

import "math"

func minimumAverageDifference(nums []int) int {
	for i := 1; i < len(nums); i++ {
		nums[i] = nums[i-1] + nums[i]
	}
	minIdx := 0
	min := math.MaxInt
	count := len(nums) - 1
	for i := 0; i <= len(nums)-1; i++ {
		avgLow := nums[i] / (i + 1)
		avgHigh := 0
		if i+1 < len(nums) {
			avgHigh = (nums[count] - nums[i]) / (len(nums) - (i + 1))
		}
		diff := abs(avgLow - avgHigh)
		if diff < min {
			min = diff
			minIdx = i
		}
	}
	return minIdx
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```
