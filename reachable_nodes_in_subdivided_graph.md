# Reachable Nodes In Subdivided Graph

## Metadata

- **Date Solved:** April 30, 2023 3:59 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/reachable-nodes-in-subdivided-graph/description/
- **Algorithm:** bfs, dijkstra, heap
- **Type:** graph

## Problem Statement

You are given an undirected graph (the "original graph") with n nodes labeled from 0 to n - 1. You decide to subdivide each edge in the graph into a chain of nodes, with the number of new nodes varying between each edge.
The graph is given as a 2D array of edges where edges[i] = [ui, vi, cnti] indicates that there is an edge between nodes ui and vi in the original graph, and cnti is the total number of new nodes that you will subdivide the edge into. Note that cnti == 0 means you will not subdivide the edge.
To subdivide the edge [ui, vi], replace it with (cnti + 1) new edges and cnti new nodes. The new nodes are x1, x2, ..., xcnti, and the new edges are [ui, x1], [x1, x2], [x2, x3], ..., [xcnti-1, xcnti], [xcnti, vi].
In this new graph, you want to know how many nodes are reachable from the node 0, where a node is reachable if the distance is maxMoves or less.
Given the original graph and maxMoves, return the number of nodes that are reachable from node 0 in the new graph.

## Constraints

- 0 <= edges.length <= min(n * (n - 1) / 2, 104)
- edges[i].length == 3
- 0 <= ui < vi < n
- There are no multiple edges in the graph.
- 0 <= cnti <= 104
- 0 <= maxMoves <= 109
- 1 <= n <= 3000

## Problem

- **Status:** Looked At Discuss


```go
package main

import "container/heap"

type Nodes [][2]int

func (n Nodes) Len() int              { return len(n) }
func (n Nodes) Less(i, j int) bool    { return n[i][1] < n[j][1] }
func (n Nodes) Swap(i, j int)         { n[i], n[j] = n[j], n[i] }
func (n *Nodes) Push(obj interface{}) { *n = append(*n, obj.([2]int)) }
func (n *Nodes) Pop() interface{} {
	old := *n
	l := len(old)
	x := old[l-1]
	*n = old[0 : l-1]
	return x
}

func reachableNodes(edges [][]int, maxMoves int, n int) int {
	distance := make(map[int]int, n)
	adjList := make([][][2]int, n)
	for _, edge := range edges {
		adjList[edge[0]] = append(adjList[edge[0]], [2]int{edge[1], edge[2]})
		adjList[edge[1]] = append(adjList[edge[1]], [2]int{edge[0], edge[2]})
	}

	distance[0] = 0
	pq := &Nodes{[2]int{0, 0}}
	heap.Init(pq)
	for pq.Len() > 0 {
		no := heap.Pop(pq).([2]int)
		u := no[0]
		dist := no[1]
		for _, n := range adjList[u] {
			v := n[0]
			weight := n[1]
			if d, exists := distance[v]; !exists || d > dist+weight+1 {
				distance[v] = dist + weight + 1
				heap.Push(pq, [2]int{v, distance[v]})
			}
		}
	}

	ret := 0
	for i := 0; i < n; i++ {
		if d, ok := distance[i]; ok && d <= maxMoves {
			ret++
		} else {
			distance[i] = maxMoves + 1
		}
	}
	for _, edge := range edges {
		x := max(0, maxMoves-distance[edge[0]])
		y := max(0, maxMoves-distance[edge[1]])
		ret += min(edge[2], x+y)
	}
	return ret
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
