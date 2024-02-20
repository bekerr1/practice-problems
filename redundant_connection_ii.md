# Redundant Connection II

## Metadata

- **Date Solved:** February 19, 2024 7:07 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/redundant-connection-ii/description/
- **Algorithm:** dfs, union find
- **Type:** graph

## Problem Statement

In this problem, a rooted tree is a directed graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with n nodes (with distinct values from 1 to n), with one additional directed edge added. The added edge has two different vertices chosen from 1 to n, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [ui, vi] that represents a directed edge connecting nodes ui and vi, where ui is a parent of child vi.

Return an edge that can be removed so that the resulting graph is a rooted tree of nnodes. If there are multiple answers, return the answer that occurs last in the given 2D-array.

## Constraints

- n == edges.length
- 3 <= n <= 1000
- edges[i].length == 2
- 1 <= ui, vi <= n
- ui != vi

## Solution

- **Status:** Looked At Discuss

### Solution Notes

This question is pretty tough but actually once the initiation is understood its pretty simple to rationalize.

There are three cases to address. Two cases involve where a node has two parents and one case involves an every node having a http://parent.In. In the third case, the extra edges is the one to the root, as the root node should have 0 parents.

In the first two cases, we need to select either of the edges to remove. In order to pick the correct one, we save the index two both possible edges and when performing union find, we skip this edge. By doing this, if we encounter a cycle while skipping this edges, we know that we've chosen the right edge to remove.

If we have two possible edges that can be removed, we remove the laster one in the list. This is achieved by keeping the two edges and establishing an order between them. 

So in summary, in case 1 & 2, we decide which edges is the right one to choose by skipping union find on one edge and determining if we still encounter a cycle. In this way we are able to 'test' which edges is the right one to remove.


```go
package main

var N int

func findRedundantDirectedConnection(edges [][]int) []int {
	N = len(edges)

	var e1, e2 int = -1, -1
	inDegree := make([]int, N+1)
	for i := 0; i < len(edges); i++ {
		if inDegree[edges[i][1]] != 0 {
			e2 = inDegree[edges[i][1]] - 1
			e1 = i
			break
		}
		inDegree[edges[i][1]] = i + 1
	}

	parents = make([]int, N+1)
	rank = make([]int, N+1)
	for i := 0; i < len(edges); i++ {
		// Skip the first edges that makes a cycle
		if i == e1 {
			continue
		}

		u, v := edges[i][0], edges[i][1]
		if !union(u, v) {
			if e1 != -1 {
				return edges[e2]
			}
			return edges[i]
		}
	}
	return edges[e1]
}

var parents []int
var rank []int

func union(n1, n2 int) bool {
	if parents == nil {
		parents = make([]int, N+1)
	}
	if rank == nil {
		rank = make([]int, N+1)
	}

	n1p := find(n1)
	n2p := find(n2)
	if n1p == n2p {
		return false
	}
	if rank[n1p] < rank[n2p] {
		parents[n1p] = n2p
	} else if rank[n1p] > rank[n2p] {
		parents[n2p] = n1p
	} else {
		rank[n1p]++
		parents[n2p] = n1p
	}
	return true
}

func find(n int) int {
	if parents[n] == n || parents[n] == 0 {
		return n
	}
	parents[n] = find(parents[n])
	return parents[n]
}
```
