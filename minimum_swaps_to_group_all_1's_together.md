# Minimum Swaps to Group All 1's Together

## Metadata

- **Date Solved:** January 7, 2024 10:14 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-swaps-to-group-all-1s-together/description/?envType=study-plan-v2&envId=amazon-spring-23-high-frequency
- **Algorithm:** slidingwindow
- **Type:** array

## Problem Statement

Given a binary array data, return the minimum number of swaps required to group all 1’s present in the array together in any place in the array.

## Constraints

- 1 <= data.length <= 105
- data[i] is either 0 or 1.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Key intuition here was to realize the best possible subarray you could create would be of size X where x is the count of 1's in the array. You could imagine if all the ones were grouped together, this would result if them all being in the same subarray. From here, you would create a window of size X and slide the window along the array. If you pre-calculate the number of 1's and 0's in the first window, there is no need to recalculate as it slides. Just account for the exiting index and entering index. 


```go
package main

import "math"

func minSwaps(data []int) int {
	totalones := 0
	left, right := 0, 0
	digitCount := [2]int{}
	for _, d := range data {
		if d == 1 {
			totalones++
			digitCount[data[right]]++
			right++
		}
	}

	swaps := math.MaxInt
	for right < len(data) {
		swaps = min(swaps, totalones-digitCount[1])
		digitCount[data[left]]--
		left++
		digitCount[data[right]]++
		right++
	}
	return min(swaps, totalones-digitCount[1])
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
