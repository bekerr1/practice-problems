# Delete and Earn

## Metadata

- **Date Solved:** April 26, 2022 7:45 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/delete-and-earn/
- **Algorithm:** dynamic programming
- **Type:** array

## Problem Statement

You are given an integer array nums. You want to maximize the number of points you get by performing the following operation any number of times:
- Pick any nums[i] and delete it to earn nums[i] points. Afterwards, you must delete every element equal to nums[i] - 1 and every element equal to nums[i] + 1.
Return the maximum number of points you can earn by applying the above operation some number of times.

## Problem


```go
package main

func deleteAndEarn(nums []int) int {
	largestNum := 0
	for _, n := range nums {
		if n > largestNum {
			largestNum = n
		}
	}
	dp := make([]int, largestNum+1)
	numFrequency := make([]int, largestNum+1)
	for _, n := range nums {
		numFrequency[n]++
	}

	dp[0] = 0
	dp[1] = numFrequency[1]

	for i := 2; i <= largestNum; i++ {
		dp[i] = max(dp[i-1], dp[i-2]+(i*numFrequency[i]))
	}
	return dp[largestNum]
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
