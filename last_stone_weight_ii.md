# Last Stone Weight II

## Metadata

- **Date Solved:** October 3, 2022 10:23 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/last-stone-weight-ii/
- **Algorithm:** dynamic programming
- **Type:** 0/1 Knapsack, array

## Problem Statement

You are given an array of integers stones where stones[i] is the weight of the ith stone.
We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights x and y with x <= y. The result of this smash is:
- If x == y, both stones are destroyed, and
- If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.
At the end of the game, there is at most one stone left.
Return the smallest possible weight of the left stone. If there are no stones left, return 0.

## Constraints

- 1 <= stones.length <= 30
- 1 <= stones[i] <= 100

## Solution

- **Status:** Looked At Discuss

### Solution Notes

https://joseiciano.medium.com/last-stone-weight-ii-a-leetcode-problem-a37f49e55d13


```go
package main

func lastStoneWeightII(stones []int) int {
	stoneSum := 0
	dp := [31][1501]int{}

	for _, s := range stones {
		stoneSum += s
	}

	for i := 1; i <= len(stones); i++ {
		for j := 1; j <= stoneSum/2; j++ {
			if stones[i-1] <= j {
				dp[i][j] = max(dp[i-1][j], stones[i-1]+dp[i-1][j-stones[i-1]])
			} else {
				dp[i][j] = dp[i-1][j]
			}
		}
	}
	return stoneSum - 2*dp[len(stones)][stoneSum/2]
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Statement

This was very tricky for me. The 0/1 Knapsack problems are always hard for me to grok. Thinking of this problem in terms of a mathematical solution would have helped a bit. Instead of considering each smash individually and iterating, seeing that grouping a set of smashes and realizing that two subset arrays can be formed to get the minimum value at the end of all smashes is key.
