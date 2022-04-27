# House Robber II

## Metadata

- **Date Solved:** April 26, 2022 10:16 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/house-robber-ii/
- **Algorithm:** dynamic programming
- **Type:** array

## Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.
Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

## Problem


```go
package main

func rob(nums []int) int {
	if len(nums) == 1 {
		return nums[0]
	}
	front := rob1(nums[0 : len(nums)-1])
	back := rob1(nums[1:len(nums)])
	return max(front, back)
}

func rob1(nums []int) int {
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
