# Find the Most Competitive Subsequence

## Metadata

- **Date Solved:** September 9, 2022 8:22 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-most-competitive-subsequence/
- **Algorithm:** brute force, greedy, monotonic stack
- **Type:** array

## Problem Statement

Given an integer array nums and a positive integer k, return the most competitive subsequence of nums of size k.
An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.
We define that a subsequence a is more competitive than a subsequence b (of the same length) if in the first position where a and b differ, subsequence a has a number less than the corresponding number in b. For example, [1,3,4] is more competitive than [1,3,5] because the first position they differ is at the final number, and 4 is less than 5.

## Constraints

- 1 <= nums.length <= 105
- 0 <= nums[i] <= 109
- 1 <= k <= nums.length
## Solution

### Solution Notes

Building the answer from left to right, while considering that smaller numbers to the right should be preferred over larger numbers already seen from the left.



### Brute force (TLE)

```go
package main

import "math"

func mostCompetitive(nums []int, k int) []int {
	ret := []int{}
	lastIdx := -1
	for k > 0 {
		tmpLastIdx := 0
		minNum := math.MaxInt
		for i := len(nums) - k; i > lastIdx; i-- {
			if nums[i] <= minNum {
				minNum = nums[i]
				tmpLastIdx = i
			}
		}
		ret = append(ret, minNum)
		lastIdx = tmpLastIdx
		k--
	}
	return ret

}
```

### Monotonic Stack

```go
package main

func mostCompetitive(nums []int, k int) []int {
	stack := []int{}
	for i := 0; i < len(nums); i++ {
		var top int
		for len(stack) > 0 && len(stack)+(len(nums)-i) > k {
			top = stack[len(stack)-1]
			if top <= nums[i] {
				break
			}
			stack = stack[0 : len(stack)-1]
		}
		if len(stack) < k {
			stack = append(stack, nums[i])
		}
	}
	return stack
}
```
