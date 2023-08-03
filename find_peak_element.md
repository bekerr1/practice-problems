# Find Peak Element

## Metadata

- **Date Solved:** August 3, 2023 9:32 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-peak-element/description/
- **Algorithm:** binary search
- **Type:** array

## Problem Statement

A peak element is an element that is strictly greater than its neighbors.

Given a 0-indexed integer array nums, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.

You may imagine that nums[-1] = nums[n] = -∞. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in O(log n) time.

## Constraints

- 1 <= nums.length <= 1000
- -231 <= nums[i] <= 231 - 1
- nums[i] != nums[i + 1] for all valid i.

## Solution

- **Status:** Solved On Own

### Solution Notes

Binary search where if the desired number is not the current mid, you move towards the highest number of the two adjacent ones.


```go
package main

func findPeakElement(nums []int) int {

	low, high := 0, len(nums)-1
	for low <= high {
		mid := low + (high-low)/2
		if mid-1 < 0 && mid+1 >= len(nums) ||
			(mid-1 < 0 || nums[mid-1] < nums[mid]) &&
				(mid+1 > len(nums)-1 || nums[mid+1] < nums[mid]) {
			return mid
		} else {
			if mid-1 < 0 || mid+1 < len(nums) && nums[mid+1] > nums[mid-1] {
				low = mid + 1
			} else {
				high = mid - 1
			}
		}
	}
	return -1
}
```
