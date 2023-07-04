# Special Permutations

## Metadata

- **Date Solved:** July 4, 2023 3:52 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/special-permutations/description/
- **Algorithm:** bitmask, dynamic programming
- **Type:** array

## Problem Statement

You are given a 0-indexed integer array nums containing n distinct positive integers. A permutation of nums is called special if:
- For all indexes 0 <= i < n - 1, either nums[i] % nums[i+1] == 0 or nums[i+1] % nums[i] == 0.
Return the total number of special permutations. As the answer could be large, return it modulo 109 + 7.

## Constraints

- 2 <= nums.length <= 14
- 1 <= nums[i] <= 109

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Bitmask was used to keep track of previous numbers used already. The ‘prev’ index is the last one compared against. For every recursion, compare prev to current and if the condition fits, cache current, set prev to current and increment current. For every combination we iterate through N^2. The ‘dp’ stores the state at a previously stored index for a given mask. Given [3, 6, 9, 12], if 3 yeilds a succesful mask, 6, 3 [3] should return the same for a given mask, along with 9, 3 [3] 12, 3 [3]


```go
package main

var dp []map[int]int

const mod = 1000000007

func specialPerm(nums []int) int {
	dp = make([]map[int]int, 20)
	for i := range dp {
		dp[i] = make(map[int]int)
	}
	return specialPermUtil(nums, 0, -1, 0)
}

func specialPermUtil(nums []int, mask, prev, current int) int {
	if current == len(nums) {
		return 1
	}

	if v, ok := dp[prev+1][mask]; ok {
		return v
	}

	var v int
	for i := 0; i < len(nums); i++ {
		if mask&(1<<i) > 0 {
			continue
		}

		if prev == -1 || nums[prev]%nums[i] == 0 || nums[i]%nums[prev] == 0 {
			v += specialPermUtil(nums, mask|(1<<i), i, current+1)
			v %= mod
		}
	}
	dp[prev+1][mask] = v
	return dp[prev+1][mask]
}
```
