# All Ancestors of a Node in a Directed Acyclic Graph

## Metadata

- **Date Solved:** June 29, 2024 12:09 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/all-ancestors-of-a-node-in-a-directed-acyclic-graph/description/?envType=daily-question&envId=2024-06-29
- **Algorithm:** dfs
- **Type:** graph

## Problem Statement

You are given a positive integer n representing the number of nodes of a Directed Acyclic Graph (DAG). The nodes are numbered from 0 to n - 1 (inclusive).

You are also given a 2D integer array edges, where edges[i] = [fromi, toi] denotes that there is a unidirectional edge from fromi to toi in the graph.

Return a list answer, where answer[i] is the list of ancestors of theith node, sorted in ascending order.

A node u is an ancestor of another node v if u can reach v via a set of edges.

## Constraints


- 1 <= n <= 1000
- 0 <= edges.length <= min(2000, n * (n - 1) / 2)
- edges[i].length == 2
- 0 <= fromi, toi <= n - 1
- fromi != toi
- There are no duplicate edges.
- The graph is directed and acyclic.

## Solution

- **Status:** Needed Hints


```go
package main

func getAncestors(n int, edges [][]int) [][]int {
	adjList := make([][]int, n)
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], e[1])
	}
	answers := make([][]int, n)
	var dfs func(a, u int)
	dfs = func(a, u int) {
		if a != u {
			if len(answers[u]) > 0 && answers[u][len(answers[u])-1] == a {
				return
			}
			answers[u] = append(answers[u], a)
		}
		for _, v := range adjList[u] {
			dfs(a, v)
		}
	}
	for i := 0; i < n; i++ {
		dfs(i, i)
	}
	return answers
}
```
