# Number of Subsequences That Satisfy the Given Sum Condition

## Metadata

- **Date Solved:** May 15, 2023 11:41 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/description/
- **Algorithm:** binary search, math, sort
- **Type:** array, math

## Problem Statement

You are given an array of integers nums and an integer target.
Return the number of non-empty subsequences of nums such that the sum of the minimum and maximum element on it is less or equal to target. Since the answer may be too large, return it modulo 109 + 7.

## Constraints

- 1 <= nums.length <= 105
- 1 <= nums[i] <= 106
- 1 <= target <= 106

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Binary exponentiation and memoization


```go
package main

import "sort"

const mod = 1000000007

func numSubseq(nums []int, target int) int {
	sort.Ints(nums)

	memo := make([]int, len(nums))
	memo[0] = 1
	for i := 1; i < len(memo); i++ {
		memo[i] = (2 * memo[i-1]) % mod
	}

	ret := 0
	low, high := 0, len(nums)-1
	for low <= high {
		if nums[high]+nums[low] <= target {
			//ret += pow(2, high-low)
			//ret = (ret + binpow(2, high-low)) % mod
			ret = (ret + memo[high-low]) % mod
			low++
		} else {
			high--
		}
	}
	return ret
}

func binpow(base, toThe int) int {
	ret := 1
	for toThe > 0 {
		if toThe&1 == 1 {
			ret = (ret * base) % mod
		}
		base = (base * base) % mod
		toThe = toThe >> 1
	}
	return ret
}

func pow(base, toThe int) int {
	ret := 1
	for i := 0; i < toThe; i++ {
		ret = (ret * base)
	}
	return ret
}
```
