# Find if Path Exists in Graph

## Metadata

- **Date Solved:** April 21, 2024 5:14 PM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/find-if-path-exists-in-graph/description/?envType=daily-question&envId=2024-04-21
- **Algorithm:** bfs, dfs, union find
- **Type:** graph

## Problem Statement

There is a bi-directional graph with n vertices, where each vertex is labeled from 0 to n - 1 (inclusive). The edges in the graph are represented as a 2D integer array edges, where each edges[i] = [ui, vi] denotes a bi-directional edge between vertex ui and vertex vi. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself.

You want to determine if there is a valid path that exists from vertex source to vertex destination.

Given edges and the integers n, source, and destination, return true if there is a valid path from source to destination, or falseotherwise.

## Constraints


- 1 <= n <= 2 * 105
- 0 <= edges.length <= 2 * 105
- edges[i].length == 2
- 0 <= ui, vi <= n - 1
- ui != vi
- 0 <= source, destination <= n - 1
- There are no duplicate edges.
- There are no self edges.

## Solution

- **Status:** Needed Hints


```go
package main

func validPath(n int, edges [][]int, source int, destination int) bool {
	if source == destination {
		return true
	}
	InitDSU(n)
	for _, e := range edges {
		if (e[0] == source && e[1] == destination) || (e[0] == destination && e[1] == source) {
			return true
		}
		union(e[0], e[1])
	}
	return find(source) == find(destination)
}

var parents []int
var rank []int

func InitDSU(n int) {
	parents, rank = make([]int, n), make([]int, n)
	for i := range rank {
		parents[i] = i
	}
}

func union(u, v int) {
	pu, pv := find(u), find(v)
	if pu == pv {
		return
	}
	if rank[pu] > rank[pv] {
		parents[pv] = pu
	} else if rank[pu] < rank[pv] {
		parents[pu] = pv
	} else {
		rank[pu] += rank[pv] + 1
		parents[pv] = pu
	}
}

func find(u int) int {
	if parents[u] == u {
		return u
	}
	parents[u] = find(parents[u])
	return parents[u]
}
```
