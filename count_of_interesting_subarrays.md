# Count of Interesting Subarrays

## Metadata

- **Date Solved:** September 9, 2023 11:46 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/count-of-interesting-subarrays/description/
- **Algorithm:** hashmap, prefixsum
- **Type:** array, subarray

## Problem Statement

You are given a 0-indexed integer array nums, an integer modulo, and an integer k.
Your task is to find the count of subarrays that are interesting.

A subarray nums[l..r] is interesting if the following condition holds:

- Let cnt be the number of indices i in the range [l, r] such that nums[i] % modulo == k. Then, cnt % modulo == k.

Return an integer denoting the count of interesting subarrays.

Note: A subarray is a contiguous non-empty sequence of elements within an array.

## Constraints

- 1 <= nums.length <= 105 
- 1 <= nums[i] <= 109
- 1 <= modulo <= 109
- 0 <= k < modulo

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Create prefix sum adding up each index that is special (nums[i]%modulo == k). Then using this prefix sum, find all indicies where sum[i] % modulo == k.

The subarray would be (count[i] - count[j]) % modulo == k % modulo for i > j. Rewrite this to count[j] % modulo = (count[i] - k) % modulo


```go
package main

func countInterestingSubarrays(nums []int, modulo int, k int) int64 {
	counts := make([]int, len(nums)+1)
	counts[0] = 0
	for i := 1; i <= len(nums); i++ {
		add := 0
		if nums[i-1]%modulo == k {
			add = 1
		}
		counts[i] = counts[i-1] + add
	}

	var ans int64 = 0
	subarrays := make(map[int]int)
	for i := 0; i < len(counts); i++ {
		m := (counts[i] - k) % modulo
		ans += int64(subarrays[m])
		subarrays[counts[i]%modulo]++
	}
	return ans
}
```
