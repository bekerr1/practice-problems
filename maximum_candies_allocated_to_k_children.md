# Maximum Candies Allocated to K Children

## Metadata

- **Date Solved:** August 16, 2023 6:08 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-candies-allocated-to-k-children/description/
- **Algorithm:** binary search
- **Type:** array

## Problem Statement

You are given a 0-indexed integer array candies. Each element in the array denotes a pile of candies of size candies[i]. You can divide each pile into any number of sub piles, but you cannot merge two piles together.

You are also given an integer k. You should allocate piles of candies to k children such that each child gets the same number of candies. Each child can take at most one pile of candies and some piles of candies may go unused.

Return the maximum number of candies each child can get.

## Constraints

- 1 <= candies.length <= 105
- 1 <= candies[i] <= 107
- 1 <= k <= 1012

## Solution

- **Status:** Solved On Own


```go
package main

func maximumCandies(candies []int, k int64) int {
	var sumOfCandies int64
	maxOfCandies := 0
	for i := 0; i < len(candies); i++ {
		sumOfCandies += int64(candies[i])
		maxOfCandies = max(maxOfCandies, candies[i])
	}

	if sumOfCandies < k {
		return 0
	}

	ret := 0
	low, mid, high := 1, 0, maxOfCandies
	for low <= high {
		mid = low + (high-low)/2
		piles := 0
		for i := 0; i < len(candies); i++ {
			possiblePiles := candies[i] / mid
			piles += possiblePiles
		}
		if int64(piles) < k {
			high = mid - 1
		} else {
			ret = max(ret, mid)
			low = mid + 1
		}
	}
	return ret
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
