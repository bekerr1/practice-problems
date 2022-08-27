# Maximum Subarray

## Metadata

- **Date Solved:** August 26, 2022 11:33 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-subarray/
- **Algorithm:** divide and conquer, kadane, prefixsum
- **Type:** array

## Problem Statement

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
A subarray is a contiguous part of an array.

## Constraints

- 1 <= nums.length <= 105
- -104 <= nums[i] <= 104

## Solution


### Prefix Sum

```go
package main

import "math"

func maxSubArray(nums []int) int {
	maxSubArray := nums[0]
	for i := 1; i < len(nums); i++ {
		if maxSubArray < nums[i] {
			maxSubArray = nums[i]
		}
		nums[i] += nums[i-1]
	}
	leftMinIdx := 0
	maxOfNums := math.MinInt
	for i := 0; i < len(nums); i++ {
		maxOfNums = max(maxOfNums, nums[i])
		if nums[i] > nums[leftMinIdx] {
			maxSubArray = max(maxSubArray, nums[i]-nums[leftMinIdx])
		} else {
			leftMinIdx = i
		}
	}
	return max(maxSubArray, maxOfNums)
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Divide and Conquer

```go
package main

import "math"

func maxSubArray(nums []int) int {
	return maxSum(0, len(nums)-1, nums)
}

func maxSum(low, high int, nums []int) int {
	if low == high {
		return nums[low]
	}

	mid := low + (high-low)/2
	sum, isum := 0, math.MinInt64
	for i := mid; i >= low; i-- {
		sum += nums[i]
		if sum > isum {
			isum = sum
		}
	}
	sum, jsum := 0, math.MinInt64
	for j := mid + 1; j <= high; j++ {
		sum += nums[j]
		if sum > jsum {
			jsum = sum
		}
	}
	maxLeftRight := int(math.Max(float64(maxSum(low, mid, nums)),
		float64(maxSum(mid+1, high, nums))))
	return int(math.Max(float64(maxLeftRight), float64(isum+jsum)))
}
```

### Kadane

```go
package main

import "math"

func maxSubArray(nums []int) int {
	maxSoFar := 0
	maxSubarraySum := math.MinInt
	for i := 0; i < len(nums); i++ {
		maxSoFar += nums[i]
		if maxSoFar > maxSubarraySum {
			maxSubarraySum = maxSoFar
		}
		// Do this every time so we can restart if we go negative
		if maxSoFar < 0 {
			maxSoFar = 0
		}
	}
	return maxSubarraySum
}
```

### Kadane-ish Simplified

```go
package main

func maxSubArray(nums []int) int {
	var (
		localSum  = nums[0]
		globalSum = nums[0]
	)
	for i := 1; i < len(nums); i++ {
		localSum = max(localSum+nums[i], nums[i])
		globalSum = max(localSum, globalSum)
	}
	return globalSum
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
