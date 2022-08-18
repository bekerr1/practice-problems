# Combination Sum

## Metadata

- **Date Solved:** August 18, 2022 3:04 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/combination-sum/
- **Algorithm:** backtracking, recursion
- **Type:** array

## Problem Statement

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.
The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.
It is guaranteed that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

## Constraints

- 1 <= candidates.length <= 30
- 1 <= candidates[i] <= 200
- All elements of candidates are distinct.
- 1 <= target <= 500

## Solution


```go
package main

func combinationSum(candidates []int, target int) [][]int {
	res := make([][]int, 0)
	cur := make([]int, 0)
	combinationSumUtil(candidates, target, 0, &res, cur)
	return res
}

func combinationSumUtil(candidates []int, target, idx int, res *[][]int, cur []int) {
	if target == 0 {
		tmp := append([]int(nil), cur...)
		*res = append(*res, tmp)
		return
	}
	for i := idx; i < len(candidates) && target > 0; i++ {
		cur = append(cur, candidates[i])
		combinationSumUtil(candidates, target-candidates[i], i, res, cur)
		cur = cur[0 : len(cur)-1]
	}
	return
}
```

### Testcases

[2, 3, 5, 6, 8, 9], target = 11

[2, 2, 2, 5], [3, 3, 3, 2], [3, 3, 5]

### Statement

Here, we could not really use memoization or tabulation in the dynamic programming style because we had to keep track of the actual combinations. Caching unused combinations in memory would have been too much, so keeping a single set of the current collection and only adding verified collections to result was best. Since the values could be actual values in the collection could be used multiple times, the for loop + backtracking method combined with recursion was the best solution.
