# Find First and Last Position of Element in Sorted Array

## Metadata

- **Date Solved:** July 29, 2023 10:57 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
- **Algorithm:** binary search
- **Type:** array

## Problem Statement

Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

## Constraints

- 0 <= nums.length <= 105
- -109 <= nums[i] <= 109
- nums is a non-decreasing array.
- -109 <= target <= 109

## Solution

- **Status:** Needed Hints


```go
package main

func searchRange(nums []int, target int) []int {
	lower := binarySerachLower(nums, target)
	upper := binarySerachUpper(nums, target)
	return []int{lower, upper}
}

func binarySerachLower(nums []int, target int) int {
	ans, low, high := -1, 0, len(nums)-1
	for low <= high {
		mid := low + (high-low+1)/2
		if nums[mid] < target {
			low = mid + 1
		} else if nums[mid] > target {
			high = mid - 1
		} else {
			ans = mid
			high = mid - 1
		}
	}
	return ans
}

func binarySerachUpper(nums []int, target int) int {
	ans, low, high := -1, 0, len(nums)-1
	for low <= high {
		mid := low + (high-low+1)/2
		if nums[mid] < target {
			low = mid + 1
		} else if nums[mid] > target {
			high = mid - 1
		} else {
			ans = mid
			low = mid + 1
		}
	}
	return ans
}
```
