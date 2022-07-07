# Sort Colors

## Metadata

- **Date Solved:** July 7, 2022 7:30 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/sort-colors/
- **Algorithm:** array
- **Type:** array

## Problem Statement

Given an array nums with n objects colored red, white, or blue, sort them https://en.wikipedia.org/wiki/In-place_algorithm so that objects of the same color are adjacent, with the colors in the order red, white, and blue.
We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.
You must solve this problem without using the library's sort function.

## Problem


```go
package main

func sortColors(nums []int) {
	colors := make([]int, 3)

	for _, n := range nums {
		colors[n]++
	}

	for i, j := 0, 0; i < len(nums); i++ {
		for colors[j] == 0 {
			j++
		}
		nums[i] = j
		colors[j]--
	}
}
```
