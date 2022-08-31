# Network Delay Time

## Metadata

- **Date Solved:** August 31, 2022 7:03 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/network-delay-time/
- **Algorithm:** bfs, dijkstra
- **Type:** graph

## Problem Statement

You are given a network of n nodes, labeled from 1 to n. You are also given times, a list of travel times as directed edges times[i] = (ui, vi, wi), where ui is the source node, vi is the target node, and wi is the time it takes for a signal to travel from source to target.
We will send a signal from a given node k. Return the minimum time it takes for all the n nodes to receive the signal. If it is impossible for all the n nodes to receive the signal, return -1.

## Constraints

- 1 <= k <= n <= 100
- 1 <= times.length <= 6000
- times[i].length == 3
- 1 <= ui, vi <= n
- ui != vi
- 0 <= wi <= 100
- All the pairs (ui, vi) are unique. (i.e., no multiple edges.)

## Solution


### BFS

```go
package main

import "math"

func networkDelayTime(times [][]int, n int, k int) int {
	adjList := make([][][2]int, n+1)
	for i := range times {
		adjList[times[i][0]] = append(adjList[times[i][0]], [2]int{times[i][1], times[i][2]})
	}

	distFromSrc := make([]int, n+1)
	for i := range distFromSrc {
		distFromSrc[i] = math.MaxInt
	}
	distFromSrc[0] = 0
	distFromSrc[k] = 0

	queue := []int{}
	queue = append(queue, k)

	for len(queue) > 0 {
		cur := queue[0]
		queue = queue[1:len(queue)]

		for _, neighbor := range adjList[cur] {
			curDistance := distFromSrc[cur] + neighbor[1]
			if curDistance < distFromSrc[neighbor[0]] {
				distFromSrc[neighbor[0]] = curDistance
				queue = append(queue, neighbor[0])
			}
		}
	}

	minDistance := math.MinInt
	for i := range distFromSrc {
		minDistance = max(minDistance, distFromSrc[i])
	}
	if minDistance == math.MaxInt {
		return -1
	}
	return minDistance
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Dijkstra

```go
package main
```
