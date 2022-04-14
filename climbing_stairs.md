# Climbing Stairs

## Metadata

- **Date Solved:** April 13, 2022 10:02 PM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/climbing-stairs/
- **Algorithm:** dynamic programming
- **Type:** array

## Problem Statement

You are climbing a staircase. It takes n steps to reach the top.
Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

```go
package main

import "fmt"

func climbStairs(n int) int {
	dp := make([]int, n+1)
	dp[0] = 1
	dp[1] = 1
	for i := 2; i <= n; i++ {
		dp[i] = dp[i-1] + dp[i-2]
		fmt.Println(dp)
	}
	return dp[n]
}
```
