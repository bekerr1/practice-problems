# 3Sum

## Metadata

- **Date Solved:** July 7, 2022 3:20 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/3sum/
- **Algorithm:** two pointer
- **Type:** array, blind75, hashmap

## Problem Statement

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.
Notice that the solution set must not contain duplicate triplets.

## Problem


```go
package main

import "sort"

func threeSum(nums []int) [][]int {
	sort.Ints(nums)
	var (
		numsMap = make(map[[3]int]bool)
		next    int
	)
	for start, end := 0, len(nums)-1; start+1 < end; start++ {
		for next, end = start+1, len(nums)-1; next < end; {
			if nums[start]+nums[next]+nums[end] > 0 {
				end--
			} else if nums[start]+nums[next]+nums[end] < 0 {
				next++
			} else {
				numsMap[[3]int{nums[start], nums[next], nums[end]}] = true
				next++
			}
		}
	}
	retNums := make([][]int, 0, len(numsMap))
	for n := range numsMap {
		retNums = append(retNums, []int{n[0], n[1], n[2]})
	}
	return retNums
}
```

## Solution Statement


Intuition here was to ‘fix’ the start and shrink a window (mid, end) smaller depending on the sum value of the three indexes. We do this for every value in the array until start + 1 == end.
