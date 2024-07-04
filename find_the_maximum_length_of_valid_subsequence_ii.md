# Find the Maximum Length of Valid Subsequence II

## Metadata

- **Date Solved:** July 4, 2024 12:10 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-maximum-length-of-valid-subsequence-ii/description/
- **Algorithm:** dynamic programming
- **Type:** array, subsequence

## Problem Statement

You are given an integer array nums and a positive integer k.
A subsequence sub of nums with length x is called valid if it satisfies:

- (sub[0] + sub[1]) % k == (sub[1] + sub[2]) % k == ... == (sub[x - 2] + sub[x - 1]) % k.Return the length of the longest valid subsequence of nums.

## Constraints


- 2 <= nums.length <= 103
- 1 <= nums[i] <= 107
- 1 <= k <= 103

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Got a TLE solution on my own but could not optimize it enough to get a passing solution.

Looks like the solution should be tabulation instead of memoization. I did top-down instead of bottom-up.


### Top Down (TLE)

```go
package main

var maxk = 100001

func maximumLength(nums []int, k int) int {
	memo = make(map[[3]int]int)
	var ans int
	for i := range nums {
		ans = max(ans, solve(nums, i, i+1, maxk, k))
	}
	return ans
}

var memo map[[3]int]int

func solve(nums []int, l, h int, val, k int) int {
	if h == len(nums) {
		return 0
	}

	if a, ok := memo[[3]int{l, h, val}]; ok {
		return a
	}

	v := (nums[l] + nums[h]) % k
	var take, dont int
	if val == maxk || v == val {
		ret := 1
		if val == maxk {
			ret = 2
		}
		take = solve(nums, h, h+1, v, k) + ret
	}
	dont = solve(nums, l, h+1, val, k)
	ans := max(take, dont)
	memo[[3]int{l, h, val}] = ans
	return ans
}
```
