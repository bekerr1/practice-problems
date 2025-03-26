# Find the Closest Marked Node

## Metadata

- **Date Solved:** March 25, 2025 8:08 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-closest-marked-node/description/
- **Algorithm:** bfs, dijkstra
- **Type:** graph

## Problem Statement

You are given a positive integer n which is the number of nodes of a 0-indexed directed weighted graph and a 0-indexed 2D array edgeswhere edges[i] = [ui, vi, wi] indicates that there is an edge from node ui to node vi with weight wi.

You are also given a node s and a node array marked; your task is to find the minimum distance from s to any of the nodes in marked.

Return an integer denoting the minimum distance from s to any node in marked or -1 if there are no paths from s to any of the marked nodes.

## Constraints


- 2 <= n <= 500
- 1 <= edges.length <= 104
- edges[i].length = 3
- 0 <= edges[i][0], edges[i][1] <= n - 1
- 1 <= edges[i][2] <= 106
- 1 <= marked.length <= n - 1
- 0 <= s, marked[i] <= n - 1
- s != marked[i]
- marked[i] != marked[j] for every i != j
- The graph might have repeated edges.
- The graph is generated such that it has no self-loops.

## Solution

- **Status:** Solved On Own

### Solution Notes

Tracking visited does not work for all TCs because the graph can have repeated edges. This should have alerted me to that aspect and I would likely have to ask this of the interviewer. 


```go
package main

import (
	"container/heap"
	"math"
)

func minimumDistance(n int, edges [][]int, s int, marked []int) int {
	adjList := make([][][2]int, n)
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], [2]int{e[1], e[2]})
	}
	// Easy lookup for marked nodes
	markMap := make(map[int]bool)
	for i := range marked {
		markMap[marked[i]] = true
	}

	// Graph metadata
	dist := make([]int, n)
	for i := range dist {
		dist[i] = math.MaxInt
	}
	dist[s] = 0
	ans := math.MaxInt

	// PQ
	q := IntHeap{[2]int{s, 0}}
	heap.Init(&q)
	for q.Len() > 0 {
		cur := heap.Pop(&q).([2]int)
		u := cur[0]
		// Once we find a mark
		if markMap[u] {
			ans = min(ans, dist[u])
			continue
		}
		for _, vw := range adjList[u] {
			v, w := vw[0], vw[1]
			if dist[v] > dist[u]+w {
				dist[v] = dist[u] + w
				heap.Push(&q, [2]int{v, dist[v]})
			}
		}
	}
	if ans == math.MaxInt {
		return -1
	}
	return ans
}

// An IntHeap is a min-heap of ints.
type IntHeap [][2]int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i][1] < h[j][1] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x any) {
	// Push and Pop use pointer receivers because they modify the slice's length,
	// not just its contents.
	*h = append(*h, x.([2]int))
}

func (h *IntHeap) Pop() any {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}
```
