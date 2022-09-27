# Number of Ways to Reach a Position After Exactly k Steps

## Metadata

- **Date Solved:** September 26, 2022 10:52 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/number-of-ways-to-reach-a-position-after-exactly-k-steps/
- **Algorithm:** dynamic programming
- **Type:** array

## Problem Statement

You are given two positive integers startPos and endPos. Initially, you are standing at position startPos on an infinite number line. With one step, you can move either one position to the left, or one position to the right.
Given a positive integer k, return the number of different ways to reach the position endPos starting from startPos, such that you perform exactly k steps. Since the answer may be very large, return it modulo 109 + 7.
Two ways are considered different if the order of the steps made is not exactly the same.
Note that the number line includes negative integers.

## Constraints

- 1 <= startPos, endPos, k <= 1000

## Solution

- **Status:** Looked At Discuss


```go
package main

import "math"

var memo map[[2]int]int
var mod = int(math.Pow10(9)) + 7

func numberOfWays(startPos int, endPos int, k int) int {
	memo = make(map[[2]int]int)
	return numberOfWaysUtil(startPos, endPos, k)
}

func numberOfWaysUtil(pos, end int, k int) int {
	if k == 0 && pos != end {
		return 0
	} else if k == 0 && pos == end {
		return 1
	} else {
		if v, ok := memo[[2]int{pos, k}]; ok {
			return v
		}
		f := numberOfWaysUtil(pos+1, end, k-1)
		b := numberOfWaysUtil(pos-1, end, k-1)
		memo[[2]int{pos, k}] = (f + b) % mod
		return memo[[2]int{pos, k}]
	}
}
```
