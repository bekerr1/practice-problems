# Find All Numbers Disappeared in an Array

## Metadata

- **Date Solved:** September 4, 2022 10:01 AM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/
- **Algorithm:** two pass
- **Type:** array

## Problem Statement

Given an array nums of n integers where nums[i] is in the range [1, n], return an array of all the integers in the range
 [1, n] that do not appear in nums

## Constraints

- n == nums.length
- 1 <= n <= 105
- 1 <= nums[i] <= n

## Solution


```go
package main

func findDisappearedNumbers(nums []int) []int {
	for i := 0; i < len(nums); i++ {
		cur := abs(nums[i]) - 1
		if nums[cur] > 0 {
			nums[cur] = -nums[cur]
		}
	}
	ret := []int{}
	for i := 1; i <= len(nums); i++ {
		if nums[i-1] > 0 {
			ret = append(ret, i)
		}
	}
	return ret
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```
