# Largest Color Value in a Directed Graph

## Metadata

- **Date Solved:** December 10, 2023 10:51 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/largest-color-value-in-a-directed-graph/description/?envType=study-plan-v2&envId=google-spring-23-high-frequency
- **Algorithm:** bfs, dfs, topological sort
- **Type:** graph

## Problem Statement

There is a directed graph of n colored nodes and medges. The nodes are numbered from 0 to n - 1.

You are given a string colors where colors[i] is a lowercase English letter representing the color of the ith node in this graph (0-indexed). You are also given a 2D array edges where edges[j] = [aj, bj]indicates that there is a directed edge from node ajto node bj.

A valid path in the graph is a sequence of nodes x1 -> x2 -> x3 -> ... -> xk such that there is a directed edge from xi to xi+1 for every 1 <= i < k. The color value of the path is the number of nodes that are colored the most frequently occurring color along that path.

Return the largest color value of any valid path in the given graph, or -1 if the graph contains a cycle.

## Constraints

- n == colors.length
- m == edges.length
- 1 <= n <= 105
- 0 <= m <= 105
- colors consists of lowercase English letters.
- 0 <= aj, bj < n

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Keeping track of the max of all colors for every stack entry helped provide a solution here.


### DFS

```go
package main

import "math"

func largestPathValue(colors string, edges [][]int) int {
	adjList := make([][]int, len(colors))
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], e[1])
	}

	visited := make([]bool, len(colors))
	dp := make([][26]int, len(colors))
	stack := make([]bool, len(colors))

	var dfs func(int) int
	dfs = func(node int) int {
		if stack[node] {
			return math.MaxInt
		}
		if visited[node] {
			return dp[node][colors[node]-'a']
		}

		visited[node] = true
		stack[node] = true

		for _, n := range adjList[node] {
			if dfs(n) == math.MaxInt {
				return math.MaxInt
			}
			for i := 0; i < 26; i++ {
				dp[node][i] = max(dp[node][i], dp[n][i])
			}
		}

		dp[node][colors[node]-'a']++
		stack[node] = false
		return dp[node][colors[node]-'a']
	}

	var answer int
	for i := range colors {
		answer = max(answer, dfs(i))
	}

	if answer == math.MaxInt {
		return -1
	}
	return answer
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### BFS

```go
package main
```
