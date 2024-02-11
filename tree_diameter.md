# Tree Diameter

## Metadata

- **Date Solved:** February 11, 2024 5:52 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/tree-diameter/description/
- **Algorithm:** bfs, dfs
- **Type:** graph

## Problem Statement

The diameter of a tree is the number of edges in the longest path in that tree.

There is an undirected tree of n nodes labeled from 0 to n - 1. You are given a 2D array edges where edges.length == n - 1 and edges[i] = [ai, bi] indicates that there is an undirected edge between nodes ai and bi in the tree.

Return the diameter of the tree.

## Constraints

- n == edges.length + 1
- 1 <= n <= 104
- 0 <= ai, bi < n
- ai != bi

## Solution

- **Status:** Solved On Own


```go
package main

func treeDiameter(edges [][]int) int {
	N := len(edges) + 1

	// Build graph DSs
	adjList := make([][]int, N)
	refs := make([]int, N)
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], e[1])
		adjList[e[1]] = append(adjList[e[1]], e[0])
		refs[e[0]]++
		refs[e[1]]++
	}

	// Find a random leaf node to start from
	minRefNode := 0
	for i := range refs {
		if refs[minRefNode] > refs[i] {
			minRefNode = i
		}
	}

	maxCount := 0
	visited := make([]bool, N)
	var dfs func(int, int) int
	// Count is the max of the number of nodes seen before recursing
	// vs the number of nodes seen after recursing
	dfs = func(u, count int) int {
		maxCount = max(maxCount, count)
		var dist int
		for _, v := range adjList[u] {
			if !visited[v] {
				visited[v] = true
				// Dont override dist if we already recursed
				// a deeper path
				dist = max(dist, dfs(v, max(count, dist)+1))
			}
		}
		return dist + 1
	}

	// start dfs from leaf. We will traverse the whole graph
	// starting from here.
	visited[minRefNode] = true
	dfs(minRefNode, 0)
	return maxCount
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
