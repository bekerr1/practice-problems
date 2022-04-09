# Next Permutation

## Metadata

- **Date Solved:** April 9, 2022 1:45 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/next-permutation/
- **Algorithm:** two pointer
- **Type:** array

## Problem Statement

A permutation of an array of integers is an arrangement of its members into a sequence or linear order.
- For example, for arr = [1,2,3], the following are considered permutations of arr: [1,2,3], [1,3,2], [3,1,2], [2,3,1].
The next permutation of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the next permutation of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).
- For example, the next permutation of arr = [1,2,3] is [1,3,2].
- Similarly, the next permutation of arr = [2,3,1] is [3,1,2].
- While the next permutation of arr = [3,2,1] is [1,2,3] because [3,2,1] does not have a lexicographical larger rearrangement.
Given an array of integers nums, find the next permutation of nums.
The replacement must be http://en.wikipedia.org/wiki/In-place_algorithm and use only constant extra memory.

## Problem


```go
package main

// Find first number that is less than one to right
// Find first number greater than that number and swap
// Then reverse set of numbers to the right of the swaped number
// 1 2 3 -> 1 3 2 -> 2 1 3
func nextPermutation(nums []int) {
	var left int
	var right int

	for left = len(nums) - 2; left >= 0; left-- {
		if nums[left] < nums[left+1] {
			break
		}
	}
	if left >= 0 {
		for right = len(nums) - 1; right > left; right-- {
			if nums[right] > nums[left] {
				break
			}
		}
		nums[left], nums[right] = nums[right], nums[left]
	}
	left = left + 1
	right = len(nums) - 1
	for left < right {
		nums[left], nums[right] = nums[right], nums[left]
		left++
		right--
	}
}
```
