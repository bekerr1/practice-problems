# Minimum Cost for Cutting Cake II

## Metadata

- **Date Solved:** July 19, 2024 8:03 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/minimum-cost-for-cutting-cake-ii/description/
- **Algorithm:** greedy, sort
- **Type:** array

## Problem Statement

There is an m x n cake that needs to be cut into 1 x 1 pieces.
You are given integers m, n, and two arrays:

- horizontalCut of size m - 1, where horizontalCut[i] represents the cost to cut along the horizontal line i.
- verticalCut of size n - 1, where verticalCut[j] represents the cost to cut along the vertical line j.

In one operation, you can choose any piece of cake that is not yet a 1 x 1 square and perform one of the following cuts:

1. Cut along a horizontal line i at a cost of horizontalCut[i].
2. Cut along a vertical line j at a cost of verticalCut[j].

After the cut, the piece of cake is divided into two distinct pieces.
The cost of a cut depends only on the initial cost of the line and does not change.

Return the minimum total cost to cut the entire cake into 1 x 1 pieces.

## Constraints


- 1 <= m, n <= 105
- horizontalCut.length == m - 1
- verticalCut.length == n - 1
- 1 <= horizontalCut[i], verticalCut[i] <= 103

## Solution

- **Status:** Solved On Own


```go
package main

import (
	"math"
	"sort"
)

func minimumCost(m int, n int, horizontalCut []int, verticalCut []int) int64 {

	sort.Sort(sort.Reverse(sort.IntSlice(horizontalCut)))
	sort.Sort(sort.Reverse(sort.IntSlice(verticalCut)))

	ans, hcount, vcount := int64(0), 0, 0
	for i, j := 0, 0; i < len(horizontalCut) || j < len(verticalCut); {
		hcut, vcut := math.MinInt, math.MinInt
		if i < len(horizontalCut) {
			hcut = horizontalCut[i]
		}
		if j < len(verticalCut) {
			vcut = verticalCut[j]
		}
		if hcut > vcut {
			ans += int64(hcut * (vcount + 1))
			hcount++
			i++
		} else {
			ans += int64(vcut * (hcount + 1))
			vcount++
			j++
		}
	}
	return ans
}
```
