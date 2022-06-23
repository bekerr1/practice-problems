# K Radius Subarray Averages

## Metadata

- **Date Solved:** June 23, 2022 8:38 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/k-radius-subarray-averages/
- **Algorithm:** prefixsum
- **Type:** array

## Problem Statement

You are given a 0-indexed array nums of n integers, and an integer k.
The k-radius average for a subarray of nums centered at some index i with the radius k is the average of all elements in nums between the indices i - k and i + k (inclusive). If there are less than k elements before or after the index i, then the k-radius average is -1.
Build and return an array avgs of length n where avgs[i] is the k-radius average for the subarray centered at index i.
The average of x elements is the sum of the x elements divided by x, using integer division. The integer division truncates toward zero, which means losing its fractional part.

## Problem


```go
package main

func getAverages(nums []int, k int) []int {
	subarraySum := make([]int, len(nums)+1)

	subarraySum[0] = 0
	for i := 1; i <= len(nums); i++ {
		subarraySum[i] = subarraySum[i-1] + nums[i-1]
	}

	for i := 0; i < len(nums); i++ {
		if i-k >= 0 && i+k < len(nums) {
			sum := subarraySum[i+k+1] - subarraySum[i-k]
			nums[i] = sum / ((k * 2) + 1)
		} else {
			nums[i] = -1
		}
	}
	return nums
}
```
