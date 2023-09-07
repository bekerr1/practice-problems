# Minimum Operations to Form Subsequence With Target Sum

## Metadata

- **Date Solved:** September 6, 2023 10:36 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/minimum-operations-to-form-subsequence-with-target-sum/description/
- **Algorithm:** bit manipulation, greedy
- **Type:** array, target

## Problem Statement

You are given a 0-indexed array nums consisting of non-negative powers of 2, and an integer target.
In one operation, you must apply the following changes to the array:

- Choose any element of the array nums[i] such that nums[i] > 1.
- Remove nums[i] from the array.
- Add two occurrences of nums[i] / 2 to the end of nums.

Return the minimum number of operations you need to perform so that nums contains a subsequence whose elements sum to target. If it is impossible to obtain such a subsequence, return -1.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

## Constraints

- 1 <= nums.length <= 1000
- 1 <= nums[i] <= 230
- nums consists only of non-negative powers of two.
- 1 <= target < 231

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Had some of the right concepts in play. I was not iterating on the target bits the right way and i had no plan on how to calculate operations well.


```go
package main

import "math"

func minOperations(nums []int, target int) int {
	baseTwoCount := [32]int{}
	for _, n := range nums {
		baseTwoCount[int(math.Log2(float64(n)))]++
	}

	ops, j := 0, 32
	for i := 0; i < 31; i++ {
		if target&(1<<i) > 0 {
			if baseTwoCount[i] > 0 {
				baseTwoCount[i]--
			} else {
				j = min(i, j)
			}
		}
		if baseTwoCount[i] > 0 && j < i {
			ops += i - j
			j = 32
			baseTwoCount[i]--
		}
		baseTwoCount[i+1] += baseTwoCount[i] / 2
	}
	if j < 32 {
		return -1
	}
	return ops
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
