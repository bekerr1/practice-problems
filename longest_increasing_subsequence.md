# Longest Increasing Subsequence

## Metadata

- **Date Solved:** July 31, 2023 8:51 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/longest-increasing-subsequence/description/
- **Algorithm:** binary search, dynamic programming
- **Type:** array

## Problem Statement

Given an integer array nums, return the length of the longest strictly increasing subsequence

## Constraints

- 1 <= nums.length <= 2500
- -104 <= nums[i] <= 104

## Solution

- **Status:** Looked At Discuss


### Dynamic Programming

```go
package main

func lengthOfLIS(nums []int) int {
	dp := make([]int, len(nums))
	ret := 0
	for i := 0; i < len(nums); i++ {
		dp[i] = 1
		for j := 0; j < i; j++ {
			if nums[i] > nums[j] && dp[i] < dp[j]+1 {
				dp[i] = dp[j] + 1
			}
		}
		ret = max(ret, dp[i])
	}
	return ret
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Binary Search

```go
package main

func lengthOfLIS(nums []int) int {
	lis := make([]int, 0)
	for _, n := range nums {
		if len(lis) == 0 || lis[len(lis)-1] < n {
			lis = append(lis, n)
		} else {
			replaceIdx := binarySearchIdx(lis, n)
			lis[replaceIdx] = n
		}
	}
	return len(lis)
}

func binarySearchIdx(nums []int, target int) int {
	ret, low, high := 0, 0, len(nums)-1
	for low <= high {
		mid := low + (high-low)/2
		if nums[mid] == target {
			return mid
		} else if nums[mid] < target {
			low = mid + 1
		} else {
			ret = mid
			high = mid - 1
		}
	}
	return ret
}
```
