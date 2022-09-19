# Uncrossed Lines

## Metadata

- **Date Solved:** September 19, 2022 9:04 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/uncrossed-lines/
- **Algorithm:** dynamic programming
- **Type:** array

## Problem Statement

You are given two integer arrays nums1 and nums2. We write the integers of nums1 and nums2 (in the order they are given) on two separate horizontal lines.

## Constraints

- 1 <= nums1.length, nums2.length <= 500
- 1 <= nums1[i], nums2[j] <= 2000

## Solution

- **Status:** Looked At Discuss


```go
package main

func maxUncrossedLines(nums1 []int, nums2 []int) int {
	dp := [501][501]int{}
	for i := 1; i <= len(nums1); i++ {
		for j := 1; j <= len(nums2); j++ {
			if nums1[i-1] == nums2[j-1] {
				dp[i][j] = dp[i-1][j-1] + 1
			} else {
				dp[i][j] = max(dp[i-1][j], dp[i][j-1])
			}
		}
	}
	return dp[len(nums1)][len(nums2)]
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
