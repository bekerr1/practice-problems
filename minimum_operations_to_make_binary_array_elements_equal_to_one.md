# Minimum Operations to Make Binary Array Elements Equal to One

## Metadata

- **Date Solved:** March 19, 2025 6:50 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-operations-to-make-binary-array-elements-equal-to-one-i/description/?envType=daily-question&envId=2025-03-19
- **Algorithm:** slidingwindow
- **Type:** array

## Problem Statement

You are given a binary array nums.

You can do the following operation on the array any number of times (possibly zero):

- Choose any 3 consecutive elements from the array and flip all of them.

Flipping an element means changing its value from 0 to 1, and from 1 to 0.

Return the minimum number of operations required to make all elements in nums equal to 1. If it is impossible, return -1.

## Constraints


- 3 <= nums.length <= 105
- 0 <= nums[i] <= 1

## Solution

- **Status:** Needed Hints


```go
package main

func minOperations(nums []int) int {
	ans := 0
	for low := 0; low < len(nums); low++ {
		if nums[low] == 0 {
			if low+3 <= len(nums) {
				nums[low] ^= 1
				nums[low+1] ^= 1
				nums[low+2] ^= 1
				ans++
			}
		}
	}
	for _, n := range nums {
		if n == 0 {
			return -1
		}
	}
	return ans
}
```
