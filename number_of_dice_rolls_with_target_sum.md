# Number of Dice Rolls With Target Sum

## Metadata

- **Date Solved:** February 6, 2024 11:32 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/number-of-dice-rolls-with-target-sum/
- **Algorithm:** dynamic programming
- **Type:** scenario

## Problem Statement

You have n dice, and each dice has k faces numbered from 1 to k.

Given three integers n, k, and target, return the number of possible ways (out of the kn total ways) to roll the dice, so the sum of the face-up numbers equals target. Since the answer may be too large, return it modulo 109 + 7.

## Constraints

- 1 <= n, k <= 30
- 1 <= target <= 1000

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Had the correct structure here. Just implemented the wrong base case.


```go
package main

var memo map[[2]int]int

func numRollsToTarget(n int, k int, target int) int {
	memo = make(map[[2]int]int)
	return solve(0, n, k, 0, target)
}

var mod = 1000000007

func solve(n, maxN, k int, count, target int) int {
	if n == maxN {
		if count == target {
			return 1
		}
		return 0
	}

	if v, ok := memo[[2]int{n, count}]; ok {
		return v
	}

	var ans int
	for i := 1; i <= k; i++ {
		ans += solve(n+1, maxN, k, count+i, target)
		ans %= mod
	}
	memo[[2]int{n, count}] = ans
	return memo[[2]int{n, count}]
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
