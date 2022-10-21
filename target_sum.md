# Target Sum

## Metadata

- **Date Solved:** October 21, 2022 4:41 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/target-sum/
- **Algorithm:** backtracking, dynamic programming
- **Type:** array

## Problem Statement

You are given an integer array nums and an integer target.
You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.
- For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".
Return the number of different expressions that you can build, which evaluates to target.

## Constraints

- 1 <= nums.length <= 20
- 0 <= nums[i] <= 1000
- 0 <= sum(nums[i]) <= 1000
- -1000 <= target <= 1000

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Not the first time I have seen a dynamic programming problem that can be solved in this manner. Hopefully going forward its easier to identify these and solve them
Also see: https://leetcode.com/problems/target-sum/discuss/455024/DP-IS-EASY!-5-Steps-to-Think-Through-DP-Questions.


```go
package main

var memo map[[2]int]int

func findTargetSumWays(nums []int, target int) int {
	memo = make(map[[2]int]int, target)
	return findTargetSumWaysUtil(nums, target, 0, 0)
}

func findTargetSumWaysUtil(nums []int, target, idx, count int) int {

	if val, exists := memo[[2]int{idx, count}]; exists {
		return val
	}

	if idx == len(nums) {
		if target == count {
			return 1
		}
		return 0
	}

	pos := findTargetSumWaysUtil(nums, target, idx+1, count+nums[idx])
	neg := findTargetSumWaysUtil(nums, target, idx+1, count-nums[idx])

	memo[[2]int{idx, count}] = pos + neg
	return memo[[2]int{idx, count}]
}
```
