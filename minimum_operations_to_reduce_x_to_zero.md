# Minimum Operations to Reduce X to Zero

## Metadata

- **Date Solved:** June 19, 2022 7:53 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/
- **Algorithm:** slidingwindow, two pointer
- **Type:** array

## Problem Statement

You are given an integer array nums and an integer x. In one operation, you can either remove the leftmost or the rightmost element from the array nums and subtract its value from x. Note that this modifies the array for future operations.
Return the minimum number of operations to reduce x to exactly 0 if it is possible, otherwise, return -1.

## Problem


```go
package main

func minOperations(nums []int, x int) int {
	var (
		count     = len(nums)
		ans   int = 1000000

		start     int
		end       int
		sum       int
		windowSum int
	)

	for _, n := range nums {
		sum += n
	}

	xDiff := sum - x
	for end < count {
		windowSum += nums[end]
		for start <= end && windowSum > xDiff {
			windowSum -= nums[start]
			start++
		}
		if windowSum == xDiff {
			ans = min(ans, count-(end-start+1))
		}
		end++
	}
	if ans == 1000000 {
		return -1
	} else {
		return ans
	}
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

## Solution Statements


The intuition here was to maintain a sliding window where the window sum is compared to the total sum of the numbers minus the target number. this way, if the window sum was equal to the target number difference, we could be assured the sum of the remaining numbers equal the target number and the total number minus the end - start + 1 would be the amount of numbers that equal the target. We would move along in this way, taking the min obtained.
