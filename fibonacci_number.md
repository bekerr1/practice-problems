# Fibonacci Number

## Metadata

- **Date Solved:** April 9, 2022 10:33 PM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/fibonacci-number/
- **Algorithm:** dynamic programming
- **Type:** math

## Problem Statement

The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,

F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.

Given n, calculate F(n).

## Problem


```go
package main

func fib(n int) int {
	if n == 0 {
		return 0
	} else if n == 1 {
		return 1
	} else {
		dp := make([]int, n+1)
		dp[0] = 0
		dp[1] = 1
		for i := 2; i <= n; i++ {
			dp[i] = dp[i-1] + dp[i-2]
		}
		return dp[n]
	}
}
```
