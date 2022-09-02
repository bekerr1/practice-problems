# Find the Smallest Divisor Given a Threshold

## Metadata

- **Date Solved:** September 2, 2022 10:47 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/
- **Algorithm:** binary search, brute force
- **Type:** array

## Problem Statement

Given an array of integers nums and an integer threshold, we will choose a positive integer divisor, divide all the array by it, and sum the division's result. Find the smallest divisor such that the result mentioned above is less than or equal to threshold.
Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: 7/3 = 3 and 10/2 = 5).
The test cases are generated so that there will be an answer.

## Constraints

- 1 <= nums.length <= 5 * 104
- 1 <= nums[i] <= 106
- nums.length <= threshold <= 106

## Solution


### Brute Force (TLE)

```go
package main

import "math"

func smallestDivisor(nums []int, threshold int) int {
	maxOfNums := 0
	for _, n := range nums {
		if n > maxOfNums {
			maxOfNums = n
		}
	}
	smallestDiv := maxOfNums + 1
	for divisor := maxOfNums; divisor >= 1; divisor-- {
		divisedSum := 0
		for _, n := range nums {
			divisedSum += divide(n, divisor)
		}
		if divisedSum <= threshold {
			smallestDiv = min(smallestDiv, divisor)
		} else {
			return smallestDiv
		}
	}
	return smallestDiv
}

func divide(num, den int) int {
	return int(math.Ceil(float64(num) / float64(den)))
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

### Binary Search

```go
package main

import "math"

func smallestDivisor(nums []int, threshold int) int {
	smallestDiv := 0
	for _, n := range nums {
		if n > smallestDiv {
			smallestDiv = n
		}
	}
	low, high := 1, smallestDiv
	for low <= high {
		mid := low + (high-low)/2
		divisedSum := 0
		for _, n := range nums {
			divisedSum += divide(n, mid)
		}
		if divisedSum <= threshold {
			smallestDiv = min(smallestDiv, mid)
			high = mid - 1
		} else {
			low = mid + 1
		}
	}
	return smallestDiv
}

func divide(num, den int) int {
	return int(math.Ceil(float64(num) / float64(den)))
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
