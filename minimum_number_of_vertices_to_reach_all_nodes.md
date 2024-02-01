# Minimum Number of Vertices to Reach All Nodes

## Metadata

- **Date Solved:** January 31, 2024 9:03 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-number-of-vertices-to-reach-all-nodes/description/
- **Algorithm:** topological sort
- **Type:** graph

## Problem Statement

Given a directed acyclic graph, with n vertices numbered from 0 to n-1, and an array edges where edges[i] = [fromi, toi] represents a directed edge from node fromi to node toi.

Find the smallest set of vertices from which all nodes in the graph are reachable. It's guaranteed that a unique solution exists.

Notice that you can return the vertices in any order.

## Constraints

- 2 <= n <= 10^5
- 1 <= edges.length <= min(10^5, n * (n - 1) / 2)
- edges[i].length == 2
- 0 <= fromi, toi < n
- All pairs (fromi, toi) are distinct.

## Solution

- **Status:** Solved On Own

### Solution Notes

Not fully topo sort, but the min number of nodes we MUST visit are at-least all the nodes with no references to other nodes as you would need to start there. 


```go
package main

func findSmallestSetOfVertices(n int, edges [][]int) []int {

	refCount := make([]int, n)
	for _, e := range edges {
		refCount[e[1]]++
	}

	ans := []int{}
	for n, rc := range refCount {
		if rc == 0 {
			ans = append(ans, n)
		}
	}
	return ans
}
```
