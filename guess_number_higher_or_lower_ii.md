# Guess Number Higher or Lower II

## Metadata

- **Date Solved:** July 26, 2024 12:10 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/guess-number-higher-or-lower-ii/
- **Algorithm:** dynamic programming
- **Type:** array, scenario

## Problem Statement

We are playing the Guessing Game. The game will work as follows:

1. I pick a number between 1 and n.
2. You guess a number.
3. If you guess the right number, you win the game.
4. If you guess the wrong number, then I will tell you whether the number I picked is higher or lower, and you will continue guessing.
5. Every time you guess a wrong number x, you will pay x dollars. If you run out of money, you lose the game.

Given a particular n, return the minimum amount of money you need to guarantee a win regardless of what number I pick.

## Constraints


- 1 <= n <= 200

## Solution

- **Status:** Looked At Discuss


```go
package main

import "math"

var dp [201][201]int

func getMoneyAmount(n int) int {
	dp = [201][201]int{}
	return solve(1, n)
}

func solve(low, high int) int {
	if low >= high {
		return 0
	}
	if dp[low][high] != 0 {
		return dp[low][high]
	}
	dp[low][high] = math.MaxInt
	for i := low; i <= high; i++ {
		dp[low][high] = min(dp[low][high], i+max(solve(low, i-1), solve(i+1, high)))
	}
	return dp[low][high]
}
```
