# Continuous Subarray Sum

## Metadata

- **Date Solved:** June 25, 2022 3:32 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/continuous-subarray-sum/
- **Algorithm:** prefixsum
- **Type:** array, hashmap

## Problem Statement

Given an integer array nums and an integer k, return true if nums has a continuous subarray of size at least two whose elements sum up to a multiple of k, or false otherwise.
An integer x is a multiple of k if there exists an integer n such that x = n * k. 0 is always a multiple of k.

## Problem


```go
package main

func checkSubarraySum(nums []int, k int) bool {
	lookup := make(map[int]int)
	lookup[0] = -1
	sum := 0
	for i := 0; i < len(nums); i++ {
		sum += nums[i]
		if ii, ok := lookup[sum%k]; ok {
			if i-ii >= 2 {
				return true
			} else {
				continue
			}
		}
		lookup[sum%k] = i
	}
	return false
}
```

## Solution Statement


Intuition here was to use the mod operator to detect when a sum was introduced that results in the mod operator returning 0 given the absence of the introduced value. An example is:

23, 2, 4, 5, 9 with k = 6

23 % 6 = 5. Since 23 is not divisible by 6, we store 5 in a map

map[5:1]

25 % 6 = 1 which is not in the map

map[5:0, 1:1]

29%6 = 5 which is in the map

Then we take the current i and subtract it from the map value to ensure we have atlest two values

2 - 0 ≥ 2

(23 + 6) % 6 is the same as 23 % 6 and 6 % 6 as 6 % 6 = 0, meaning 29 % 6 == 23 % 6
