# Maximize Total Cost of Alternating Subarrays

## Metadata

- **Date Solved:** June 29, 2024 8:42 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximize-total-cost-of-alternating-subarrays/description/
- **Algorithm:** dynamic programming
- **Type:** array

## Problem Statement

You are given an integer array nums with length n.
The cost of a subarray nums[l..r], where 0 <= l <= r < n, is defined as:

cost(l, r) = nums[l] - nums[l + 1] + ... + nums[r] * (−1)r − l
Your task is to split nums into subarrays such that the total cost of the subarrays is maximized, ensuring each element belongs to exactly one subarray.

Formally, if nums is split into k subarrays, where k > 1, at indices i1, i2, ..., ik − 1, where 0 <= i1 < i2 < ... < ik - 1 < n - 1, then the total cost will be:

cost(0, i1) + cost(i1 + 1, i2) + ... + cost(ik − 1 + 1, n − 1)
Return an integer denoting the maximum total cost of the subarrays after splitting the array optimally.

Note: If nums is not split into subarrays, i.e. k = 1, the total cost is simply cost(0, n - 1).

## Constraints


- 1 <= nums.length <= 105
- -109 <= nums[i] <= 109

## Solution

- **Status:** Needed Hints


```go
package main

func maximumTotalCost(nums []int) int64 {
	if len(nums) < 2 {
		return int64(nums[0])
	}
	dp := [2][2]int64{}
	dp[0][0] = int64(nums[0] + nums[1])
	dp[0][1] = int64(nums[0] - nums[1])
	for i := 2; i < len(nums); i++ {
		dp[1][0] = max(dp[0][0], dp[0][1]) + int64(nums[i])
		dp[1][1] = dp[0][0] - int64(nums[i])

		dp[0][0], dp[0][1] = dp[1][0], dp[1][1]
	}
	return max(dp[0][0], dp[0][1])
}
```
