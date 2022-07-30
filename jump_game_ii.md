# Jump Game II

## Metadata

- **Date Solved:** July 30, 2022 1:20 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/jump-game-ii/
- **Algorithm:** dynamic programming, greedy
- **Type:** array

## Problem Statement

Given an array of non-negative integers nums, you are initially positioned at the first index of the array.
Each element in the array represents your maximum jump length at that position.
Your goal is to reach the last index in the minimum number of jumps.
You can assume that you can always reach the last index.

## Problem


### Brute Force

```go
package main

import (
	"fmt"
	"math"
)

func jump(nums []int) int {
	dp := make([]int, len(nums))
	for i := range dp {
		dp[i] = math.MaxInt
	}

	dp[0] = 0
	for i := 0; i < len(nums); i++ {
		j := i + 1
		jumps := nums[i]
		for jumps > 0 && j < len(nums) {
			dp[j] = min(dp[j], dp[i]+1)
			jumps--
			j++
		}
		if j == len(nums) {
			fmt.Println(dp)
			return dp[j-1]
		}
	}
	return -1
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

### Optimized

```go
package main

func jump(nums []int) int {
	current, jumps, furthest := 0, 0, 0

	for i := 0; i < len(nums)-1; i++ {
		furthest = max(furthest, nums[i]+i)

		if i == current {
			current = furthest
			jumps++
		}
	}
	return jumps
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

## Solution Statement

