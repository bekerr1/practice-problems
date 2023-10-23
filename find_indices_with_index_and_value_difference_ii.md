# Find Indices With Index and Value Difference II

## Metadata

- **Date Solved:** October 23, 2023 6:20 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-indices-with-index-and-value-difference-ii/description/
- **Algorithm:** greedy
- **Type:** array

## Problem Statement

You are given a 0-indexed integer array nums having length n, an integer indexDifference, and an integer valueDifference.

Your task is to find two indices i and j, both in the range [0, n - 1], that satisfy the following conditions:

- abs(i - j) >= indexDifference, and
- abs(nums[i] - nums[j]) >= valueDifference

Return an integer array answer, where answer = [i, j] if there are two such indices, and answer = [-1, -1] otherwise. If there are multiple choices for the two indices, return any of them.

Note: i and j may be equal.

## Constraints

- 1 <= n == nums.length <= 105
- 0 <= nums[i] <= 109
- 0 <= indexDifference <= 105
- 0 <= valueDifference <= 109

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Tried to solve this using binary search at first. My method would not work in the end as the decision to go left or right would be exposed given the answer could be in either quadrant. Next time need to see this short coming much sooner to avoid implementing a wrong solution.

In the actual solution, trying to solve for the max difference between numbers was a good course to take. This coupled with ensuring the index difference was always met by starting with it in the for loop made for a good final solution.


```go
package main

func findIndices(nums []int, indexDifference int, valueDifference int) []int {
	minIdx, maxIdx := 0, 0
	for i := indexDifference; i < len(nums); i++ {
		if nums[i-indexDifference] < nums[minIdx] {
			minIdx = i - indexDifference
		}
		if nums[i-indexDifference] > nums[maxIdx] {
			maxIdx = i - indexDifference
		}
		if abs(nums[i]-nums[minIdx]) >= valueDifference {
			return []int{minIdx, i}
		}
		if abs(nums[maxIdx]-nums[i]) >= valueDifference {
			return []int{i, maxIdx}
		}
	}
	return []int{-1, -1}
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```
