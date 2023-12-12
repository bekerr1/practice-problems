# Maximum Number of K-Divisible Components

## Metadata

- **Date Solved:** December 11, 2023 10:19 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/maximum-number-of-k-divisible-components/description/
- **Algorithm:** dfs
- **Type:** binary tree, graph

## Problem Statement

There is an undirected tree with n nodes labeled from 0 to n - 1. You are given the integer n and a 2D integer array edges of length n - 1, where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the tree.

You are also given a 0-indexed integer array valuesof length n, where values[i] is the value associated with the ith node, and an integer k.

A valid split of the tree is obtained by removing any set of edges, possibly empty, from the tree such that the resulting components all have values that are divisible by k, where the value of a connected component is the sum of the values of its nodes.

Return the maximum number of components in any valid split.

## Constraints

- 1 <= n <= 3 * 104
- edges.length == n - 1
- edges[i].length == 2
- 0 <= ai, bi < n
- values.length == n
- 0 <= values[i] <= 109
- 1 <= k <= 109
- Sum of values is divisible by k.
- The input is generated such that edgesrepresents a valid tree.

## Solution

- **Status:** Solved On Own


```go
package main

func maxKDivisibleComponents(n int, edges [][]int, values []int, k int) int {
	ans := 0
	adjList := make([][]int, n)
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], e[1])
		adjList[e[1]] = append(adjList[e[1]], e[0])
	}

	var dfs func(int, int) int
	dfs = func(node, parent int) int {
		var sum int
		for _, n := range adjList[node] {
			if n != parent {
				sum += dfs(n, node)
			}
		}
		if (sum+values[node])%k == 0 {
			ans++
			return 0
		}
		return sum + values[node]
	}

	dfs(0, -1)
	return ans
}
```
