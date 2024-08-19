# Find K-th Smallest Pair Distance

## Metadata

- **Date Solved:** August 18, 2024 9:51 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/find-k-th-smallest-pair-distance/description/?envType=daily-question&envId=2024-08-14
- **Algorithm:** binary search, slidingwindow, sort
- **Type:** array

## Problem Statement

The distance of a pair of integers a and b is defined as the absolute difference between a and b.

Given an integer array nums and an integer k, return the kth smallest distance among all the pairs nums[i] and nums[j] where 0 <= i < j < nums.length.

## Constraints


- n == nums.length
- 2 <= n <= 104
- 0 <= nums[i] <= 106
- 1 <= k <= n * (n - 1) / 2

## Solution

- **Status:** Looked At Discuss

### Solution Notes

The size of K rules out using a heap. Because the solution space for nums is relatively small,  we can binary search on it and use sliding window to find the last set of nums that meets the constraints for a given target.


```go
package main

import "sort"

func smallestDistancePair(nums []int, k int) int {
	sort.Ints(nums)
	left, right := 0, nums[len(nums)-1]
	for left <= right {
		mid := left + (right-left)/2
		if KSmallerCount(mid, nums, k) {
			left = mid + 1
		} else {
			right = mid - 1
		}
	}
	return left
}

func KSmallerCount(target int, nums []int, k int) bool {
	count := 0
	right := 0
	for left := 0; left < len(nums); left++ {
		for right < len(nums) && nums[right]-nums[left] <= target {
			right++
		}
		count += right - left - 1
	}
	return count < k
}
```
