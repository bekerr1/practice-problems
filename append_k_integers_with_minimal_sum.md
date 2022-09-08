# Append K Integers With Minimal Sum

## Metadata

- **Date Solved:** September 8, 2022 8:38 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/append-k-integers-with-minimal-sum/
- **Algorithm:** brute force
- **Type:** array

## Problem Statement

You are given an integer array nums and an integer k. Append k unique positive integers that do not appear in nums to nums such that the resulting total sum is minimum.
Return the sum of the k integers appended to nums.

## Constraints

- 1 <= nums.length <= 105
- 1 <= nums[i] <= 109
- 1 <= k <= 108

## Solution

- **Status:** Solved On Own

### Solution Notes

n * (n+1)/2 can be used to calculate the sum of number from 1 to n


### Sorting

```go
package main

import "sort"

func minimalKSum(nums []int, k int) int64 {
	sort.Ints(nums)
	var i, j int
	var ret int64
	for i, j = 1, 0; k > 0; i++ {
		if j < len(nums) && i == nums[j] {
			for j < len(nums) && i == nums[j] {
				j++
			}
			continue
		}
		ret += int64(i)
		k--
	}
	return ret
}
```

### Sorting Optimized

```go
package main

import "sort"

func minimalKSum(nums []int, k int) int64 {
	sort.Ints(nums)
	var count, prevNum int
	for i := 0; i < len(nums); i++ {
		if nums[i] <= k && nums[i] != prevNum {
			prevNum = nums[i]
			count += nums[i]
			k++
		}
	}
	return int64((k * (k + 1) / 2) - count)
}
```

### Hashing

```go
package main

func minimalKSum(nums []int, k int) int64 {
	ret := int64(k * (k + 1) / 2)
	numsAboveK := make(map[int]struct{})
	numsRemovedBelowK := make(map[int]struct{})
	numsRemoved := 0
	for _, n := range nums {
		_, exists := numsRemovedBelowK[n]
		if n <= k && !exists {
			ret -= int64(n)
			numsRemoved++
			numsRemovedBelowK[n] = struct{}{}
		} else {
			numsAboveK[n] = struct{}{}
		}
	}

	for i := k + 1; numsRemoved > 0; i++ {
		if _, exists := numsAboveK[i]; !exists {
			ret += int64(i)
			numsRemoved--
		}
	}
	return ret
}
```

### Statement

The goal here was to calculate the ‘k’ least numbers that don’t exist in nums array and sum them. In the first attempt, the algorithm would iterate from 1 till whenever k == 0. If we see that num[j] == i, we dont take the i’th number and iterate on j. If the number doesn’t exist or we reached the end of nums, we will take all the i’s. In the case k was much larger than the largest number in nums, this would take more time O(k).
