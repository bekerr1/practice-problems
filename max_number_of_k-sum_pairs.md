# Max Number of K-Sum Pairs

## Metadata

- **Date Solved:** December 17, 2023 5:58 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/max-number-of-k-sum-pairs/description/
- **Algorithm:** hashmap
- **Type:** array

## Problem Statement

You are given an integer array nums and an integer k.

In one operation, you can pick two numbers from the array whose sum equals k and remove them from the array.

Return the maximum number of operations you can perform on the array.

## Constraints

- 1 <= nums.length <= 105
- 1 <= nums[i] <= 109
- 1 <= k <= 109

## Solution

- **Status:** Solved On Own

### Solution Notes

The only thing that tripped me up after my first solution was the fact that there are duplicates of a given number and if you removed that number you wouldn't remove all instances of that number. To fix this I incremented the count of the number in the hash instead of removing altogether.


```go
package main

func maxOperations(nums []int, k int) int {
	n := make(map[int]int, len(nums))
	ans := 0
	for _, num := range nums {
		if v, exists := n[k-num]; exists && v > 0 {
			ans++
			n[k-num]--
		} else {
			n[num]++
		}
	}
	return ans
}
```
