# Max Consecutive Ones III

## Metadata

- **Date Solved:** February 18, 2025 9:49 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/max-consecutive-ones-iii/description/
- **Algorithm:** slidingwindow
- **Type:** array

## Problem Statement

Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

## Constraints


- 1 <= nums.length <= 105
- nums[i] is either 0 or 1.
- 0 <= k <= nums.length

## Solution

- **Status:** Solved On Own


```go
package main

func longestOnes(nums []int, k int) int {
	zeroCount := k
	low, high := 0, 0
	ans := 0
	for high < len(nums) {
		// Go until we've hit a zero and cannot flip it
		for high < len(nums) && (zeroCount > 0 || nums[high] == 1) {
			if nums[high] == 0 {
				zeroCount--
			}
			high++
		}

		// Record this boundary if its larger than previous observed
		ans = max(ans, high-low)

		// We've reached our max flippable bits.
		// Close the window to gain more
		for high < len(nums) && zeroCount == 0 {
			if nums[low] == 0 {
				zeroCount++
			}
			low++
		}
	}
	return ans
}
```
