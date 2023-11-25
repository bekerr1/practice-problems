# Minimum Height Trees

## Metadata

- **Date Solved:** November 25, 2023 4:00 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-height-trees/description/
- **Algorithm:** bfs, topological sort
- **Type:** graph

## Problem Statement

A tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.
Given a tree of n nodes labelled from 0 to n - 1, and an array of n - 1 edges where edges[i] = [ai, bi] indicates that there is an undirected edge between the two nodes ai and bi in the tree, you can choose any node of the tree as the root. When you select a node x as the root, the result tree has height h. Among all possible rooted trees, those with minimum height (i.e. min(h))  are called minimum height trees (MHTs).

Return a list of all MHTs' root labels. You can return the answer in any order.

The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

## Constraints

- 1 <= n <= 2 * 104
- edges.length == n - 1
- 0 <= ai, bi < n
- ai != bi
- All the pairs (ai, bi) are distinct.
- The given input is guaranteed to be a tree and there will be no repeated edges.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Intuition here was that the minimum height tree would be the node(s) in the middle of the graph. So in topological sort fashion, start from the edges of the graph and work inward towards the middle. The answer would be the last nodes you see in the BFS.


```go
package main

func findMinHeightTrees(n int, edges [][]int) []int {
	if n == 0 {
		return []int{}
	}
	if n == 1 {
		return []int{0}
	}
	adjList := make([][]int, n)
	refs := make([]int, n)
	for _, edge := range edges {
		adjList[edge[0]] = append(adjList[edge[0]], edge[1])
		adjList[edge[1]] = append(adjList[edge[1]], edge[0])
		refs[edge[0]]++
		refs[edge[1]]++
	}

	queue := []int{}
	for i, n := range adjList {
		if len(n) == 1 {
			queue = append(queue, i)
		}
	}
	ans := []int{}
	for len(queue) > 0 {
		qlen := len(queue)
		ans = []int{}
		for i := 0; i < qlen; i++ {
			curr := queue[0]
			queue = queue[1:]
			ans = append(ans, curr)
			for _, e := range adjList[curr] {
				refs[e]--
				if refs[e] == 1 {
					queue = append(queue, e)
				}
			}
		}
	}
	return ans
}
```
