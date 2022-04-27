# Jump Game

## Metadata

- **Date Solved:** April 26, 2022 11:16 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/jump-game/
- **Algorithm:** dynamic programming, greedy
- **Type:** array

## Problem Statement

You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.
Return true if you can reach the last index, or false otherwise.

## Problem


```go
package main

func canJump(nums []int) bool {
	if len(nums) == 1 {
		return true
	}

	step := 1
	for _, n := range nums {
		step--
		if step < 0 {
			return false
		}
		step = max(step, n)
	}
	return true
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
