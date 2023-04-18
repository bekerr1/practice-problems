# The Number of Beautiful Subsets

## Metadata

- **Date Solved:** April 17, 2023 11:35 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/the-number-of-beautiful-subsets/description/
- **Algorithm:** backtracking, dynamic programming
- **Type:** array

## Problem Statement

You are given an array nums of positive integers and a positive integer k.
A subset of nums is beautiful if it does not contain two integers with an absolute difference equal to k.
Return the number of non-empty beautiful subsets of the array nums.
A subset of nums is an array that can be obtained by deleting some (possibly none) elements from nums. Two subsets are different if and only if the chosen indices to delete are different.

## Constraints

- 1 <= nums.length <= 20
- 1 <= nums[i], k <= 1000

## Solution

- **Status:** Looked At Discuss


```go
package main

func beautifulSubsets(nums []int, k int) int {
	n := make(map[int]int)
	return beautifulSubsetsUtil(nums, k, 0, n)
}

func beautifulSubsetsUtil(nums []int, k, st int, numsAtSoln map[int]int) int {
	count := 0
	for i := st; i < len(nums); i++ {
		if numsAtSoln[nums[i]-k] == 0 && numsAtSoln[nums[i]+k] == 0 {
			numsAtSoln[nums[i]]++
			count += 1 + beautifulSubsetsUtil(nums, k, i+1, numsAtSoln)
			numsAtSoln[nums[i]]--
		}
	}
	return count
}
```
