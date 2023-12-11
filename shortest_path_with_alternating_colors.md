# Shortest Path with Alternating Colors

## Metadata

- **Date Solved:** December 10, 2023 10:19 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/shortest-path-with-alternating-colors/description/
- **Algorithm:** bfs
- **Type:** graph

## Problem Statement

You are given an integer n, the number of nodes in a directed graph where the nodes are labeled from 0 to n - 1. Each edge is red or blue in this graph, and there could be self-edges and parallel edges.
You are given two arrays redEdges and blueEdges where:

- redEdges[i] = [ai, bi] indicates that there is a directed red edge from node ai to node bi in the graph, and
- blueEdges[j] = [uj, vj] indicates that there is a directed blue edge from node uj to node vj in the graph.

Return an array answer of length n, where each answer[x] is the length of the shortest path from node 0 to node x such that the edge colors alternate along the path, or -1 if such a path does not exist.

## Constraints

- 1 <= n <= 100
- 0 <= redEdges.length, blueEdges.length <= 400
- redEdges[i].length == blueEdges[j].length == 2
- 0 <= ai, bi, uj, vj < n

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Straight forward question with a few gotchas that really caused me to not solve the problem independently

* Loading the queue with a zero value for each colored edge
* Accounting for cycles. Keeping a visited map for each color
* Keeping an adj list for each color. If we are on a current color, cycle for edges of that color and queue the opposite color for each edge.

In general, the colored edge thought stumped me. I had a hard time making sense of how to treat a the nodes vs edges.


```go
package main

const (
	red  = -1
	blue = 1
)

var colorAdjLists map[int][][]int

func shortestAlternatingPaths(n int, redEdges [][]int, blueEdges [][]int) []int {
	redAdjList := make([][]int, n)
	blueAdjList := make([][]int, n)
	for _, e := range redEdges {
		redAdjList[e[0]] = append(redAdjList[e[0]], e[1])
	}
	for _, e := range blueEdges {
		blueAdjList[e[0]] = append(blueAdjList[e[0]], e[1])
	}

	colorAdjLists = make(map[int][][]int)
	colorAdjLists[red] = redAdjList
	colorAdjLists[blue] = blueAdjList

	answer := make([]int, n)
	for i := range answer {
		answer[i] = -1
	}
	answer[0] = 0

	visited := make(map[int][]bool)
	visited[red] = make([]bool, n)
	visited[blue] = make([]bool, n)

	queue := [][2]int{{0, red}, {0, blue}}
	dist := 0
	for len(queue) > 0 {
		qlen := len(queue)
		for i := 0; i < qlen; i++ {
			node, color := queue[0][0], queue[0][1]
			queue = queue[1:]
			for _, n := range colorAdjLists[color][node] {
				if answer[n] == -1 || answer[n] > dist+1 {
					answer[n] = dist + 1
				}
				if !visited[-color][n] {
					visited[-color][n] = true
					queue = append(queue, [2]int{n, -color})
				}
			}
		}
		dist++
	}
	return answer
}
```
