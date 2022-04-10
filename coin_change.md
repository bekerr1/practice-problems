# Coin Change

## Metadata

- **Date Solved:** April 9, 2022 10:40 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/coin-change/
- **Algorithm:** dynamic programming
- **Type:** array, blind75

## Problem Statement

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.
Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.
You may assume that you have an infinite number of each kind of coin.

## Problem


```go
package main

import (
	"math"
	"sort"
)

func coinChange(coins []int, amount int) int {
	max := amount + 1
	dp := make([]int, amount+1)
	dp[0] = 0
	sort.Ints(coins)
	for i := 1; i < max; i++ {
		dp[i] = max
		for _, c := range coins {
			if i-c < 0 {
				break
			}
			if dp[i-c] != math.MaxInt32 {
				dp[i] = min(dp[i-c]+1, dp[i])
			}
		}
	}
	if dp[amount] == max {
		return -1
	}
	return dp[amount]
}

func min(i, j int) int {
	if i < j {
		return i
	}
	return j
}
```
