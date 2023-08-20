# Check if it is Possible to Split Array

## Metadata

- **Date Solved:** August 19, 2023 9:02 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/check-if-it-is-possible-to-split-array/description/
- **Algorithm:** dynamic programming, greedy
- **Type:** array

## Problem Statement

You are given an array nums of length n and an integer m. You need to determine if it is possible to split the array into n non-empty arrays by performing a series of steps.

In each step, you can select an existing array (which may be the result of previous steps) with a length of at least two and split it into two subarrays, if, for each resulting subarray, at least one of the following holds:

- The length of the subarray is one, or
- The sum of elements of the subarray is greater than or equal to m.

Return true if you can split the given array into n arrays, otherwise return false.

Note: A subarray is a contiguous non-empty sequence of elements within an array.

## Constraints

- 1 <= n == nums.length <= 100
- 1 <= nums[i] <= 100
- 1 <= m <= 200

## Solution

- **Status:** Needed Hints

### Solution Notes

I was able to somewhat solve this one but it was the more inefficient and complicated way. I had the right idea in that we generally want to DFS into segments of the array and return true if we are able to meet the conditions.

The correct memoization approach was to keep start and end index and memoize if the [2]int{start, end} pair returns true. For every start, end pair make sure the ≥ m condition is satisfied, then for start to end, cut the array in the middle and try to find a left right that worked.


### Memoization

```go
package main

func canSplitArray(nums []int, m int) bool {
	if len(nums) <= 2 {
		return true
	}
	dp = make(map[[2]int]bool)
	return canSplitArrayUtil(nums, 0, len(nums)-1, m)
}

var dp map[[2]int]bool

func canSplitArrayUtil(nums []int, low, high int, m int) bool {
	if low == high {
		return true
	}
	if !sumGreaterEqual(nums, low, high, m) {
		return false
	}
	if v, exists := dp[[2]int{low, high}]; exists {
		return v
	}

	ans := false
	for cut := low; !ans && cut < high; cut++ {
		l := canSplitArrayUtil(nums, low, cut, m)
		r := canSplitArrayUtil(nums, cut+1, high, m)
		ans = l && r
	}
	dp[[2]int{low, high}] = ans
	return ans
}

func sumGreaterEqual(nums []int, low, high, m int) bool {
	sum := 0
	for i := low; i <= high; i++ {
		sum += nums[i]
	}
	return sum >= m
}
```

### Greedy

```go
package main
```
