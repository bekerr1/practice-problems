# Maximum Total Importance of Roads

## Metadata

- **Date Solved:** June 28, 2024 11:45 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-total-importance-of-roads/description/?envType=daily-question&envId=2024-06-28
- **Algorithm:** greedy, sort, topological sort
- **Type:** graph

## Problem Statement

You are given an integer n denoting the number of cities in a country. The cities are numbered from 0 to n - 1.
You are also given a 2D integer array roads where roads[i] = [ai, bi]denotes that there exists a bidirectional road connecting cities ai and bi.

You need to assign each city with an integer value from 1 to n, where each value can only be used once. The importance of a road is then defined as the sum of the values of the two cities it connects.
Return the maximum total importance of all roads possible after assigning the values optimally.

## Constraints


- 2 <= n <= 5 * 104
- 1 <= roads.length <= 5 * 104
- roads[i].length == 2
- 0 <= ai, bi <= n - 1
- ai != bi
- There are no duplicate roads.

## Solution

- **Status:** Solved On Own

### Solution Notes

We want to maximize importance to large numbers need to be assigned to nodes with more references and small numbers to nodes with.


```go
package main

import "sort"

func maximumImportance(n int, roads [][]int) int64 {
	refs := make([]int, n)
	for _, r := range roads {
		refs[r[0]]++
		refs[r[1]]++
	}

	sort.Ints(refs)

	var sum int64
	importance := 1
	for i := range refs {
		sum += int64(importance * refs[i])
		importance++
	}
	return sum
}
```
