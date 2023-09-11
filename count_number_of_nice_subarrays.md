# Count Number of Nice Subarrays

## Metadata

- **Date Solved:** September 10, 2023 10:36 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/count-number-of-nice-subarrays/description/
- **Algorithm:** hashmap, prefixsum
- **Type:** array, subarray

## Problem Statement

Given an array of integers nums and an integer k. A continuous subarray is called nice if there are k odd numbers on it.

Return the number of nice sub-arrays.

## Constraints

- 1 <= nums.length <= 50000
- 1 <= nums[i] <= 10^5
- 1 <= k <= nums.length

## Solution

- **Status:** Solved On Own


```go
package main

func numberOfSubarrays(nums []int, k int) int {
	odds, ans := 0, 0
	subarrays := make(map[int]int)
	subarrays[0]++
	for _, n := range nums {
		if n%2 != 0 {
			odds++
		}
		s := odds - k
		if s >= 0 {
			ans += subarrays[s]
		}
		subarrays[odds]++
	}
	return ans
}
```
