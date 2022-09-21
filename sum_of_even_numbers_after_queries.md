# Sum of Even Numbers After Queries

## Metadata

- **Date Solved:** September 21, 2022 7:13 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/sum-of-even-numbers-after-queries/
- **Algorithm:** brute force
- **Type:** array

## Problem Statement

You are given an integer array nums and an array queries where queries[i] = [vali, indexi].
For each query i, first, apply nums[indexi] = nums[indexi] + vali, then print the sum of the even values of nums.
Return an integer array answer where answer[i] is the answer to the ith query.

## Constraints

- 1 <= nums.length <= 104
- -104 <= nums[i] <= 104
- 1 <= queries.length <= 104
- -104 <= vali <= 104
- 0 <= indexi < nums.length

## Solution

- **Status:** Solved On Own


```go
package main

func sumEvenAfterQueries(nums []int, queries [][]int) []int {
	evenSum := 0
	for i := range nums {
		if nums[i]%2 == 0 {
			evenSum += nums[i]
		}
	}

	ret := make([]int, 0, len(queries))
	for i := 0; i < len(queries); i++ {
		query := queries[i]
		numAtQuery := nums[query[1]]
		if numAtQuery%2 == 0 {
			evenSum -= numAtQuery
		}
		newNumAtQuery := numAtQuery + query[0]
		if newNumAtQuery%2 == 0 {
			evenSum += newNumAtQuery
		}
		nums[query[1]] = newNumAtQuery
		ret = append(ret, evenSum)
	}
	return ret
}
```
