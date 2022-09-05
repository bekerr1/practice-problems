# Missing Number

## Metadata

- **Date Solved:** September 5, 2022 1:54 AM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/missing-number/
- **Algorithm:** two pass
- **Type:** array

## Problem Statement

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

## Constraints

- n == nums.length
- 1 <= n <= 104
- 0 <= nums[i] <= n
- All the numbers of nums are unique.

## Solution


```go
package main

func missingNumber(nums []int) int {
	var i int
	for i = 0; i < len(nums); {
		if nums[i] == len(nums) {
			i++
			continue
		}
		if nums[nums[i]] == nums[i] {
			i++
		} else {
			nums[nums[i]], nums[i] = nums[i], nums[nums[i]]
		}
	}

	for i = 0; i < len(nums); i++ {
		if nums[i] != i {
			break
		}
	}
	return i
}
```
