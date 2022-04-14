# House Robber I

## Metadata

- **Date Solved:** April 13, 2022 9:52 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/house-robber/
- **Algorithm:** dynamic programming
- **Type:** array

## Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

## Problem


```go
package main

func rob(nums []int) int {
	if len(nums) == 1 {
		return nums[0]
	}

	dp := make([]int, len(nums))
	dp[0] = nums[0]
	dp[1] = max(nums[0], nums[1])

	var i int
	for i = 2; i < len(nums); i++ {
		dp[i] = max(dp[i-1], dp[i-2]+nums[i])
	}
	return dp[i-1]
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
