# Reorder Routes to Make All Paths Lead to the City Zero

## Metadata

- **Date Solved:** September 13, 2022 10:31 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/
- **Algorithm:** bfs, dfs
- **Type:** graph

## Problem Statement

There are n cities numbered from 0 to n - 1 and n - 1 roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.
Roads are represented by connections where connections[i] = [ai, bi] represents a road from city ai to city bi.
This year, there will be a big event in the capital (city 0), and many people want to travel to this city.
Your task consists of reorienting some roads such that each city can visit the city 0. Return the minimum number of edges changed.
It's guaranteed that each city can reach city 0 after reorder.

## Constraints

- 2 <= n <= 5 * 104
- connections.length == n - 1
- connections[i].length == 2
- 0 <= ai, bi <= n - 1
- ai != bi

## Solution

- **Status:** Looked At Discuss

### Solution Notes

all the connections will span outward from ‘0’. We can use this to our advantage


### DFS

```go
package main

var adjList [][][2]int
var visited []bool

func minReorder(n int, connections [][]int) int {
	visited = make([]bool, n)
	adjList = make([][][2]int, n)
	for i := 0; i < len(connections); i++ {
		//forward
		adjList[connections[i][0]] = append(adjList[connections[i][0]], [2]int{connections[i][1], 1})
		//backward
		adjList[connections[i][1]] = append(adjList[connections[i][1]], [2]int{connections[i][0], 0})
	}
	visited[0] = true
	return dfs(0)
}

func dfs(src int) int {
	var cnt int
	for _, n := range adjList[src] {
		neighbor := n[0]
		if visit := visited[neighbor]; visit {
			continue
		}
		direction := n[1]
		visited[neighbor] = true
		cnt += dfs(neighbor) + direction
	}
	return cnt
}
```
