# Maximum Subarray Sum with One Deletion

## Metadata

- **Date Solved:** September 16, 2022 7:12 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-subarray-sum-with-one-deletion/
- **Algorithm:** brute force, dynamic programming, kadane
- **Type:** array

## Problem Statement

Given an array of integers, return the maximum sum for a non-empty subarray (contiguous elements) with at most one element deletion. In other words, you want to choose a subarray and optionally delete one element from it so that there is still at least one element left and the sum of the remaining elements is maximum possible.
Note that the subarray needs to be non-empty after deleting one element.

## Constraints

- 1 <= arr.length <= 105
- -104 <= arr[i] <= 104

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Solving using brute force then trying to simplify with kadane or DP. A good thought would be keeping separate variables for seperate use cases, one variable for non deletes and one variable for deletes.


### Brute Force

```go
package main

import "math"

func maximumSum(arr []int) int {
	if len(arr) == 1 {
		return arr[0]
	}
	maxTotal := math.MinInt
	for i := 0; i < len(arr); i++ {
		maxLeft := left(i-1, 0, arr)
		maxRight := right(i+1, len(arr), arr)
		center := arr[i]
		maxSides := max(maxLeft+maxRight, max(maxLeft, maxRight))
		maxAll := max(maxSides, maxSides+center)
		maxTotal = max(maxAll, maxTotal)
	}
	return maxTotal
}

func left(st, end int, arr []int) int {
	if st < 0 {
		return math.MinInt / 2
	}
	maxTemp, maxLeft := 0, math.MinInt
	for i := st; i >= end; i-- {
		maxTemp += arr[i]
		if maxTemp > maxLeft {
			maxLeft = maxTemp
		}
	}
	return maxLeft
}

func right(st, end int, arr []int) int {
	if st > len(arr)-1 {
		return math.MinInt / 2
	}
	maxTemp, maxRight := 0, math.MinInt
	for i := st; i < end; i++ {
		maxTemp += arr[i]
		if maxTemp > maxRight {
			maxRight = maxTemp
		}
	}
	return maxRight
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Kadane

```go
package main

import "math"

func maximumSum(arr []int) int {
	if len(arr) == 1 {
		return arr[0]
	}
	noDeleteSum, deleteSum := arr[0], 0
	ret := math.MinInt
	for i := 1; i < len(arr); i++ {
		// If our sum with no deletions is greater than with deletion
		// the sum with deletion is overwritten. This means we will,
		// in the end, only decide to take the sum with deletion if
		// the deletion did us any good.
		if noDeleteSum > deleteSum+arr[i] {
			deleteSum = noDeleteSum
		} else {
			deleteSum += arr[i]
		}

		// Either take the sum with the next element or start from
		// this element.
		if noDeleteSum+arr[i] > arr[i] {
			noDeleteSum += arr[i]
		} else {
			noDeleteSum = arr[i]
		}
		ret = max(ret, max(noDeleteSum, deleteSum))
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

### Dynamic Programming

```go
package main

func maximumSum(arr []int) int {
	ret := arr[0]
	dp := make([][2]int, len(arr))
	// This DP contains no deletes. We will always either take
	// arr[i] or dp[i-1][0] + arr[i], depending on which is larger
	dp[0][0] = arr[0]
	// This DP containers the one delete. We use the DP with no
	// deletes to take either a delete at 'i' or a delete that
	// happened at a previous 'i'
	dp[0][1] = 0
	for i := 1; i < len(arr); i++ {
		// max(current arr num, previous nums + current)
		dp[i][0] = max(arr[i], dp[i-1][0]+arr[i])
		// max(delete 'i', previous 'i' was deleted)
		dp[i][1] = max(dp[i-1][0], dp[i-1][1]+arr[i])
		ret = max(ret, max(dp[i][0], dp[i][1]))
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
