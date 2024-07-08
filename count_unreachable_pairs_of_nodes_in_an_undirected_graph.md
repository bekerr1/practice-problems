# Count Unreachable Pairs of Nodes in an Undirected Graph

## Metadata

- **Date Solved:** July 8, 2024 12:20 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/count-unreachable-pairs-of-nodes-in-an-undirected-graph/description/
- **Algorithm:** bfs, dfs, union find
- **Type:** graph

## Problem Statement

You are given an integer n. There is an undirected graph with n nodes, numbered from 0 to n - 1. You are given a 2D integer array edgeswhere edges[i] = [ai, bi] denotes that there exists an undirectededge connecting nodes ai and bi.

Return the number of pairs of different nodes that are unreachablefrom each other.

## Constraints


- 1 <= n <= 105
- 0 <= edges.length <= 2 * 105
- edges[i].length == 2
- 0 <= ai, bi < n
- ai != bi
- There are no repeated edges.

## Solution

- **Status:** Needed Hints

### Solution Notes

Calculating the final answer gave me much more trouble than it should have


```go
package main

func countPairs(n int, edges [][]int) int64 {
	initDS(n)
	for _, e := range edges {
		union(e[0], e[1])
	}

	ans := int64(0)
	compSize := make(map[int]int)
	for i := 0; i < n; i++ {
		compSize[find(i)]++
	}

	numNodes := n
	for _, v := range compSize {
		ans += int64(v * (numNodes - v))
		numNodes -= v
	}
	return ans
}

var p []int
var rank []int

func initDS(n int) {
	p, rank = make([]int, n), make([]int, n)
	for i := range p {
		p[i] = i
		rank[i]++
	}
}

func union(u, v int) {
	pu, pv := find(u), find(v)
	if pu == pv {
		return
	}
	repNode := -1
	if rank[pu] >= rank[pv] {
		p[pv] = pu
		repNode = pu
	} else {
		p[pu] = pv
		repNode = pv
	}
	rank[repNode]++
}

func find(u int) int {
	if u == p[u] {
		return u
	}
	p[u] = find(p[u])
	return p[u]
}
```
