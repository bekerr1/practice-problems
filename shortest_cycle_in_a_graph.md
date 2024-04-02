# Shortest Cycle in a Graph

## Metadata

- **Date Solved:** April 1, 2024 9:44 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/shortest-cycle-in-a-graph/description/
- **Algorithm:** bfs, dfs
- **Type:** cycle detection, graph

## Problem Statement

There is a bi-directional graph with n vertices, where each vertex is labeled from 0 to n - 1. The edges in the graph are represented by a given 2D integer array edges, where edges[i] = [ui, vi] denotes an edge between vertex ui and vertex vi. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself.

Return the length of the shortest cycle in the graph. If no cycle exists, return -1.

A cycle is a path that starts and ends at the same node, and each edge in the path is used only once.

## Constraints


- 2 <= n <= 1000
- 1 <= edges.length <= 1000
- edges[i].length == 2
- 0 <= ui, vi < n
- ui != vi
- There are no repeated edges.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

BFS only works if you do cycle detection on every node as you need to be in the cycle for the distance tracking to add up correctly.


### BFS

```go
package main

import "math"

func findShortestCycle(n int, edges [][]int) int {
	adjList := make([][]int, n)
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], e[1])
		adjList[e[1]] = append(adjList[e[1]], e[0])
	}

	ans := math.MaxInt
	dist := make([]int, n)
	queue := []int{}
	for i := 0; i < n; i++ {
		for i := range dist {
			dist[i] = -1
		}
		queue = append(queue, i)
		dist[i] = 0

		for len(queue) > 0 {
			u := queue[0]
			queue = queue[1:]
			for _, v := range adjList[u] {
				if dist[v] < 0 {
					dist[v] = dist[u] + 1
					queue = append(queue, v)
				} else if dist[v] >= dist[u] {
					ans = min(ans, dist[v]+dist[u]+1)
				}
			}
		}
	}
	if ans == math.MaxInt {
		return -1
	}
	return ans
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

### DFS

```go
package main

import "math"

func findShortestCycle(n int, edges [][]int) int {
	adjList := make([][]int, n)
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], e[1])
		adjList[e[1]] = append(adjList[e[1]], e[0])
	}

	var visited []int
	ans := math.MaxInt
	var dfs func(int, int, int)
	dfs = func(p, u, d int) {
		visited[u] = d
		for _, v := range adjList[u] {
			if visited[v] == 0 {
				dfs(u, v, d+1)
			} else if v != p {
				ans = min(ans, abs(d-visited[v])+1)
				if visited[v] > d+1 {
					visited[v] = d + 1
				}
			}
		}
	}
	for i := 0; i < n; i++ {
		visited = make([]int, n)
		dfs(-1, i, 1)
	}
	if ans == math.MaxInt {
		return -1
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
