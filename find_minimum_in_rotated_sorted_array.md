# Find Minimum in Rotated Sorted Array

## Metadata

- **Date Solved:** July 6, 2022 7:20 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
- **Algorithm:** binary search
- **Type:** array, blind75

## Problem Statement

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:
- [4,5,6,7,0,1,2] if it was rotated 4 times.
- [0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].
Given the sorted rotated array nums of unique elements, return the minimum element of this array.
You must write an algorithm that runs in O(log n) time.

## Problem


```go
package main

func findMin(nums []int) int {
	low, high := 0, len(nums)-1
	for low < high {
		mid := low + (high-low)/2
		if nums[mid] < nums[low] {
			high = mid
		} else if nums[mid] > nums[high] {
			low = mid + 1
		} else {
			high = mid
		}
	}
	return nums[low]
}
```
