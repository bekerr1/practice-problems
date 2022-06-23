# Subarray Sum Equals K

## Metadata

- **Date Solved:** June 22, 2022 8:48 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/subarray-sum-equals-k/
- **Algorithm:** prefixsum
- **Type:** array, hashmap

## Problem Statement

Given an array of integers nums and an integer k, return the total number of subarrays whose sum equals to k.
A subarray is a contiguous non-empty sequence of elements within an array.

## Problem


```go
package main

func subarraySum(nums []int, k int) int {
	var (
		sum        int
		ans        int
		sumHistory = make(map[int]int)
	)
	for i := 0; i < len(nums); i++ {
		sum += nums[i]
		if sum == k {
			ans++
		}
		if amt, ok := sumHistory[sum-k]; ok {
			ans += amt
		}
		sumHistory[sum]++
	}
	return ans
}
```

## Solution Statement


Prefix/Suffix sums are used to easily calculate the sum of contiguous subarrays. The intuition behind this problem is given at any point in the prefix sum array, if you find prefix[i] - prefix[j] = k where i > j, you an also reformat the equation as prefix[i] - k = prefix[j]. So if

1, 2, 3, 5, 6, -6 is the array and k = 5

1, 3, 6, 11, 17, 11 is the prefix sum array

if i = 2 and j = 0, prefix[i] - prefix[j] = k. To visualize this

1, 3, 6

- 1

=

3, 6 == 2, 3

This means the subarray nums[j+1:i] sums to k

To make this an O(n) problem, we will store the previous prefixSums seen in a map and increment the value at that sum by 1. This way, if we find a key in the map at map[prefix[i] - k], we know that there is a subarray from prefix[i] - (prefix[i] - k) whose sum = k
