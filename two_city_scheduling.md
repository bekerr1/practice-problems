# Two City Scheduling

## Metadata

- **Date Solved:** September 1, 2022 9:01 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/two-city-scheduling/
- **Algorithm:** dynamic programming, greedy, recursion
- **Type:** array, knapsack

## Problem Statement

A company is planning to interview 2n people. Given the array costs where costs[i] = [aCosti, bCosti], the cost of flying the ith person to city a is aCosti, and the cost of flying the ith person to city b is bCosti.
Return the minimum cost to fly every person to a city such that exactly n people arrive in each city.

## Constraints

- 2 * n == costs.length
- 2 <= costs.length <= 100
- costs.length is even.
- 1 <= aCosti, bCosti <= 1000

## Solution


### Recursive (TLE)

```go
package main

var n int

const OOBCost = 10000

func twoCitySchedCost(costs [][]int) int {
	n = len(costs) / 2
	return twoCitySchedCostUtil(costs, 0, 0, 0)
}

func twoCitySchedCostUtil(costs [][]int, idx, cityACount, cityBCount int) int {
	if idx == len(costs) && cityACount == n && cityBCount == n {
		return 0
	} else {
		if cityACount > n || cityBCount > n {
			return OOBCost
		}
	}
	c1 := twoCitySchedCostUtil(costs, idx+1, cityACount+1, cityBCount) + costs[idx][0]
	c2 := twoCitySchedCostUtil(costs, idx+1, cityACount, cityBCount+1) + costs[idx][1]
	return min(c1, c2)
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

### Dynamic Programming (Translated from above)

```go
package main

var n int
var dp [101][51][51]int

const OOBCost = 100001

func twoCitySchedCost(costs [][]int) int {
	n = len(costs) / 2
	dp = [101][51][51]int{}
	return twoCitySchedCostUtil(costs, 0, 0, 0)
}

func twoCitySchedCostUtil(costs [][]int, idx, cityAUsed, cityBUsed int) int {
	if idx == len(costs) && cityAUsed == n && cityBUsed == n {
		return 0
	} else {
		if idx == len(costs) || cityAUsed > n || cityBUsed > n {
			return OOBCost
		}
	}
	if dp[idx][cityAUsed][cityBUsed] != 0 {
		return dp[idx][cityAUsed][cityBUsed]
	}
	c1 := twoCitySchedCostUtil(costs, idx+1, cityAUsed+1, cityBUsed) + costs[idx][0]
	c2 := twoCitySchedCostUtil(costs, idx+1, cityAUsed, cityBUsed+1) + costs[idx][1]
	dp[idx][cityAUsed][cityBUsed] = min(c1, c2)
	return dp[idx][cityAUsed][cityBUsed]
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

### Greedy

```go
package main

import "sort"

func twoCitySchedCost(costs [][]int) int {
	sort.Slice(costs, func(i, j int) bool {
		return costs[i][0]-costs[i][1] < costs[j][0]-costs[j][1]
	})

	minCost := 0
	for i := 0; i < len(costs); i++ {
		if i < len(costs)/2 {
			minCost += costs[i][0]
		} else {
			minCost += costs[i][1]
		}
	}
	return minCost
}
```

### Statement

I had the intuition in my mind about the a[0] - a[1] < b[0] - b[1] but did not think to use this condition in a sort to get the answer. This was a miss on my part. The recursive solution was easy to come up with after I looked at the Knapsack link, but the DP solution is still unknown to me
