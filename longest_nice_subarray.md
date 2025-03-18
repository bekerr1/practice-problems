# Longest Nice Subarray

## Metadata

- **Date Solved:** March 17, 2025 11:59 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/longest-nice-subarray/description/?envType=daily-question&envId=2025-03-18
- **Algorithm:** bit manipulation, slidingwindow
- **Type:** array

## Problem Statement

You are given an array nums consisting of positive integers.
We call a subarray of nums nice if the bitwise AND of every pair of elements that are in different positions in the subarray is equal to 0.

Return the length of the longest nice subarray.

A subarray is a contiguous part of an array.

Note that subarrays of length 1 are always considered nice.

## Constraints


- 1 <= nums.length <= 105
- 1 <= nums[i] <= 109

## Solution

- **Status:** Solved On Own


```go
package main

func longestNiceSubarray(nums []int) int {
	if len(nums) <= 1 {
		return len(nums)
	}
	low, high := 0, 0
	xored := 0
	ans := 0
	for high < len(nums) {
		if nums[high]&xored == 0 {
			xored ^= nums[high]
			high++
		} else {
			ans = max(ans, high-low)
			for low < high && xored&nums[high] != 0 {
				xored ^= nums[low]
				low++
			}
		}
	}
	ans = max(ans, high-low)
	return ans
}
```
