# Array Partition

## Metadata

- **Date Solved:** August 18, 2022 2:51 PM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/array-partition/
- **Algorithm:** counting-sort, sort
- **Type:** array

## Problem Statement

Given an integer array nums
 of 2n integers, group these integers into n pairs (a1, b1), (a2, b2), ..., (an, bn) such that the sum of min(ai, bi)
 for all i is maximized. Return the maximized sum

## Solution


### Default Sorting Approach

```go
package main

import "sort"

func arrayPairSum(nums []int) int {
	sort.Ints(nums)
	max := 0
	for i := 0; i < len(nums); i += 2 {
		max += nums[i]
	}
	return max
}
```

This approach results in a time complexity of O(nlog(n)) and space complexity of O(log(n)) as the default sorting algorithm for Go is pdq sort (heuristic based sorting that uses quicksort, insertion sort, and a fallback sort, in the case, heapsort). The time complexity can be improved with constraints on the space using Counting sort. See [https://leetcode.com/problems/array-partition/solution/](https://leetcode.com/problems/array-partition/solution/) for details.
