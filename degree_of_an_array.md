# Degree of an Array

## Metadata

- **Date Solved:** August 27, 2022 2:07 PM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/degree-of-an-array/
- **Algorithm:** hashmap
- **Type:** array

## Problem Statement

Given a non-empty array of non-negative integers nums, the degree of this array is defined as the maximum frequency of any one of its elements.
Your task is to find the smallest possible length of a (contiguous) subarray of nums, that has the same degree as nums.

## Constraints

- nums.length will be between 1 and 50,000.
- nums[i] will be an integer between 0 and 49,999.

## Solution


```go
package main

import "math"

func findShortestSubArray(nums []int) int {
	maxDegrees := 0
	degrees := make(map[int][]int)
	for i, n := range nums {
		degrees[n] = append(degrees[n], i)
		if len(degrees[n]) > maxDegrees {
			maxDegrees = len(degrees[n])
		}
	}
	minSubarray := math.MaxInt
	for _, c := range degrees {
		if len(c) == maxDegrees {
			subarrayLen := (c[len(c)-1] - c[0]) + 1
			if minSubarray > subarrayLen {
				minSubarray = subarrayLen
			}
		}
	}
	return minSubarray
}
```

### Statement

At first thought this may be a hashmap + sliding window problem but then looked at the hint and realized we can keep an array of index for each unique value from nums. Then we can see from all the max arrays which ones have the min difference in the first and last index.
