# Minimum Score of a Path Between Two Cities

## Metadata

- **Date Solved:** April 8, 2023 6:45 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/description/
- **Algorithm:** bfs, dfs, union find
- **Type:** graph

## Problem Statement

You are given a positive integer n representing n cities numbered from 1 to n. You are also given a 2D array roads where roads[i] = [ai, bi, distancei] indicates that there is a bidirectional road between cities ai and bi with a distance equal to distancei. The cities graph is not necessarily connected.
The score of a path between two cities is defined as the minimum distance of a road in this path.
Return the minimum possible score of a path between cities 1 and n.
Note:
- A path is a sequence of roads between two cities.
- It is allowed for a path to contain the same road multiple times, and you can visit cities 1 and n multiple times along the path.
- The test cases are generated such that there is at least one path between 1 and n.

## Constraints

- 2 <= n <= 10^5
- 1 <= roads.length <= 10^5
- roads[i].length == 3
- 1 <= ai, bi <= n
- ai != bi
- 1 <= distancei <= 10^4
- There are no repeated edges.
- There is at least one path between 1 and n.

## Solution

- **Status:** Needed Hints

### Solution Notes

Node count is too high to do DFS for the possibility of stack overflow.


### BFS

O(n)

```go
package main

var minRoad int
var adjList [100001][][2]int

func minScore(n int, roads [][]int) int {
	minRoad = 10001
	adjList = [100001][][2]int{}
	for _, road := range roads {
		adjList[road[0]] = append(adjList[road[0]], [2]int{road[1], road[2]})
		adjList[road[1]] = append(adjList[road[1]], [2]int{road[0], road[2]})
	}

	queue := []int{}
	queue = append(queue, 1)
	visited := make(map[int]bool)
	visited[1] = true
	for len(queue) > 0 {
		qlen := len(queue)
		for i := 0; i < qlen; i++ {
			v := queue[0]
			queue = queue[1:]
			for _, u := range adjList[v] {
				minRoad = min(minRoad, u[1])
				if !visited[u[0]] {
					visited[u[0]] = true
					queue = append(queue, u[0])
				}
			}
		}
	}
	return minRoad
}

func min(x, y int) int {
	if x > y {
		return y
	}
	return x
}
```

### Union-Find

```go
package main
```
