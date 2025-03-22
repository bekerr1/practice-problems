# Count the Number of Complete Components

## Metadata

- **Date Solved:** March 22, 2025 2:33 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/count-the-number-of-complete-components/description/?envType=daily-question&envId=2025-03-22
- **Algorithm:** union find
- **Type:** graph

## Problem Statement

You are given an integer n. There is an undirected graph with n vertices, numbered from 0 to n - 1. You are given a 2D integer array edges where edges[i] = [ai, bi] denotes that there exists an undirected edge connecting vertices ai and bi.

Return the number of complete connected components of the graph.

A connected component is a subgraph of a graph in
which there exists a path between any two vertices, and no vertex of the
subgraph shares an edge with a vertex outside of the subgraph.

A connected component is said to be complete if there exists an edge between every pair of its vertices.

## Constraints


- 1 <= n <= 50
- 0 <= edges.length <= n * (n - 1) / 2
- edges[i].length == 2
- 0 <= ai, bi <= n - 1
- ai != bi
- There are no repeated edges.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

The “walk” aspect of this problem was the key. The more components in the subgraph you incorperate in the cost the smaller the number would get, because bitwise & is reductive. In this, the problem was essentially asking to compute the bitwise & of all componets in the subgraph. This is where union/find helps.


```go
package main

func initDS(n int) {
	parents = make([]int, n)
	size = make([]int, n)
	for i := range parents {
		parents[i] = i
		size[i]++
	}
}

func countCompleteComponents(n int, edges [][]int) int {
	initDS(n)
	adjList := make([][]int, n)
	for _, n := range edges {
		adjList[n[0]] = append(adjList[n[0]], n[1])
		adjList[n[1]] = append(adjList[n[1]], n[0])
		union(n[0], n[1])
	}
	verified := make(map[int]bool)
	for i := 0; i < n; i++ {
		p := find(i)
		verified[p] = true
	}
	for i := 0; i < n; i++ {
		p := find(i)
		if _, ok := verified[p]; ok {
			if size[p]-1 != len(adjList[i]) {
				delete(verified, p)
			}
		}
	}
	return len(verified)
}

var size []int
var parents []int

func union(n1, n2 int) {
	p1, p2 := find(n1), find(n2)
	if p1 == p2 {
		return
	} // already unioned
	// Just designate the lowest node number in the group
	// as the parent
	if p1 < p2 {
		parents[p2] = p1
		size[p1] += size[p2]
	} else {
		parents[p1] = p2
		size[p2] += size[p1]
	}
}

func find(n int) int {
	if parents[n] == n {
		return n
	}
	parents[n] = find(parents[n])
	return parents[n]
}
```
