# Find the City With the Smallest Number of Neighbors at a Threshold Distance

## Metadata

- **Date Solved:** July 29, 2024 9:35 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/description/?envType=daily-question&envId=2024-07-26
- **Algorithm:** dfs, dijkstra
- **Type:** graph

## Problem Statement

There are n cities numbered from 0 to n-1. Given the array edges where edges[i] = [fromi, toi, weighti] represents a bidirectional and weighted edge between cities fromi and toi, and given the integer distanceThreshold.

Return the city with the smallest number of cities that are reachable through some path and whose distance is at mostdistanceThreshold, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities i and j is equal to the sum of the edges' weights along that path.

## Constraints


- 2 <= n <= 100
- 1 <= edges.length <= n * (n - 1) / 2
- edges[i].length == 3
- 0 <= fromi < toi < n
- 1 <= weighti, distanceThreshold <= 10^4
- All pairs (fromi, toi) are distinct.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I tried to solve this by just calculating the path sum and summing the node each time. This was not enough. Needed to understand that tracking each nodes path sum was required. Thats because there are multiple ways to get to any path and without keeping that context you do a lot of extra work. Additionally, I was not continue down a path after visiting it. This was also wrong for the same reason. 

The major issue here was I just treated the problem like a standard graph one which was not the case


### DFS

```go
package main

import "math"

func findTheCity(n int, edges [][]int, distanceThreshold int) int {
	adjList := make([][][2]int, n)
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], [2]int{e[1], e[2]})
		adjList[e[1]] = append(adjList[e[1]], [2]int{e[0], e[2]})
	}

	var (
		dfs     func(int, int) int
		visited []bool
		temp    []int
	)
	dfs = func(u, dist int) int {
		d := 0
		for _, v := range adjList[u] {
			pathSum := dist + v[1]
			if temp[v[0]] > pathSum && pathSum <= distanceThreshold {
				temp[v[0]] = pathSum
				if !visited[v[0]] {
					visited[v[0]] = true
					d += dfs(v[0], pathSum) + 1
				} else {
					d += dfs(v[0], pathSum)
				}

			}
		}
		return d
	}

	var minPath, node int
	minPath = math.MaxInt
	for i := 0; i < n; i++ {
		visited = make([]bool, n)
		visited[i] = true
		temp = make([]int, n)
		for i := range temp {
			temp[i] = math.MaxInt32
		}
		path := dfs(i, 0)
		if path <= minPath {
			minPath = path
			node = i
		}
	}
	return node
}
```

### Dijkstra

```go
package main

import (
	"container/heap"
	"math"
)

type Edge struct {
	node, cost int
}

type PriorityQueue []Edge

func (pq PriorityQueue) Len() int           { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool { return pq[i].cost < pq[j].cost }
func (pq PriorityQueue) Swap(i, j int)      { pq[i], pq[j] = pq[j], pq[i] }

func (pq *PriorityQueue) Push(x interface{}) { *pq = append(*pq, x.(Edge)) }
func (pq *PriorityQueue) Pop() interface{} {
	old := *pq
	n := len(old)
	item := old[n-1]
	*pq = old[:n-1]
	return item
}

func comeOnDijkstra(cityInfo map[int][]Edge, idx, n, distanceAllowed int) int {
	temp := make([]int, n)
	for i := range temp {
		temp[i] = math.MaxInt32
	}
	temp[idx] = 0

	pq := &PriorityQueue{}
	heap.Init(pq)
	heap.Push(pq, Edge{idx, 0})

	for pq.Len() > 0 {
		costVertex := heap.Pop(pq).(Edge)
		cost, vertex := costVertex.cost, costVertex.node

		for _, it := range cityInfo[vertex] {
			pathSum := cost + it.cost
			if temp[it.node] > pathSum {
				temp[it.node] = pathSum
				heap.Push(pq, Edge{it.node, pathSum})
			}
		}
	}

	freq := 0
	for _, d := range temp {
		if d <= distanceAllowed {
			freq++
		}
	}
	return freq
}

func findTheCity(n int, edges [][]int, distanceThreshold int) int {
	cityInfo := make(map[int][]Edge)
	for _, it := range edges {
		cityInfo[it[0]] = append(cityInfo[it[0]], Edge{it[1], it[2]})
		cityInfo[it[1]] = append(cityInfo[it[1]], Edge{it[0], it[2]})
	}
	ans := math.MaxInt32
	result := -1

	for i := 0; i < n; i++ {
		path := comeOnDijkstra(cityInfo, i, n, distanceThreshold)
		if path <= ans {
			ans = path
			result = i
		}
	}

	return result
}
```
