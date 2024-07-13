# Minimum Cost to Cut a Stick

## Metadata

- **Date Solved:** July 13, 2024 12:58 AM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/minimum-cost-to-cut-a-stick/description/
- **Algorithm:** dynamic programming, sort
- **Type:** array, scenario

## Problem Statement

Given a wooden stick of length n units. The stick is labelled from 0 to n. For example, a stick of length 6 is labelled as follows:

Given an integer array cuts where cuts[i] denotes a position you should perform a cut at.

You should perform the cuts in order, you can change the order of the cuts as you wish.

The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.

Return the minimum total cost of the cuts.

## Constraints


- 2 <= n <= 106
- 1 <= cuts.length <= min(n - 1, 100)
- 1 <= cuts[i] <= n - 1
- All the integers in cuts array are distinct.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Modeling the problem generically and trying to solve it from more of an abstract standpoint might have been easier. Explanation turned out something like…

cost(0, n)
cut0, cut1, ..., cutn-2, cutn-1
say we cut at some idx x, cost could be calculated as...
cost[0, cutx) + cost[cutx, cutn)
we append 0 and n to the ends of cutsif right - left == 1, 
we return 0
because we cannot cut a peice that has no cuts
cost(left, right)
for i := left+1; i < right; i++cost(left, i) + cost(i, right) + (cuts[right] - cuts[left])base case isif right - left == 1 return math.MaxInt


```go
package main

import (
	"math"
	"sort"
)

var memo [][]int

func minCost(n int, cuts []int) int {
	M := len(cuts) + 2
	memo = make([][]int, M+1)
	fill := make([]int, M+1)
	for i := 0; i <= M; i++ {
		fill[i] = -1
	}
	for i := range memo {
		memo[i] = make([]int, M+1)
		copy(memo[i], fill)
	}
	sort.Ints(cuts)
	newCuts := []int{0}
	newCuts = append(newCuts, cuts...)
	newCuts = append(newCuts, n)

	return solve(0, len(newCuts)-1, newCuts)
}

func solve(left, right int, cuts []int) int {
	if memo[left][right] != -1 {
		return memo[left][right]
	}
	if right-left == 1 {
		return 0
	}

	ans := math.MaxInt
	for i := left + 1; i < right; i++ {
		cost := solve(left, i, cuts) + solve(i, right, cuts) + (cuts[right] - cuts[left])
		ans = min(ans, cost)
	}
	memo[left][right] = ans
	return ans
}
```
