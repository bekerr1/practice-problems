# Shortest Bridge

## Metadata

- **Date Solved:** May 23, 2023 6:34 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/shortest-bridge/description/
- **Algorithm:** bfs, dfs
- **Type:** graph

## Problem Statement

You are given an n x n binary matrix grid where 1 represents land and 0 represents water.
An island is a 4-directionally connected group of 1's not connected to any other 1's. There are exactly two islands in grid.
You may change 0's to 1's to connect the two islands to form one island.
Return the smallest number of 0's you must flip to connect the two islands.

## Constraints

- n == grid.length == grid[i].length
- 2 <= n <= 100
- grid[i][j] is either 0 or 1.
- There are exactly two islands in grid.

## Solution

- **Status:** Needed Hints

### Solution Notes

Adding one to the distance every time the BFS expands outward was the best way to keep track of distance here. In this way, we didn’t have to maintain an extra data structure to do so.


### BFS Only

```go
package main

var islandQueue [][2]int

const water int = 0
const undiscoveredLand int = 1
const discoveredLand int = 2
const boundary int = 3

var edges = [][2]int{
	[2]int{1, 0},
	[2]int{0, 1},
	[2]int{-1, 0},
	[2]int{0, -1},
}

func shortestBridge(grid [][]int) int {
	islandQueue = make([][2]int, 0)

Outer:
	for i := 0; i < len(grid); i++ {
		for j := 0; j < len(grid[0]); j++ {
			if grid[i][j] == undiscoveredLand {
				islandQueue = append(islandQueue, [2]int{i, j})
				bfsIsland(grid)
				break Outer
			}
		}
	}
	for i := 0; i < len(grid); i++ {
		for j := 0; j < len(grid[0]); j++ {
			if grid[i][j] == boundary {
				islandQueue = append(islandQueue, [2]int{i, j})
			}
		}
	}
	return bfsBoundary(grid)
}

func bfsIsland(grid [][]int) {
	for len(islandQueue) > 0 {
		cur := islandQueue[0]
		islandQueue = islandQueue[1:]
		for _, edge := range edges {
			nextX, nextY := cur[0]+edge[0], cur[1]+edge[1]
			next := [2]int{nextX, nextY}
			if nextX >= 0 && nextX < len(grid) && nextY >= 0 && nextY < len(grid[0]) {
				if grid[nextX][nextY] == undiscoveredLand {
					grid[nextX][nextY] = discoveredLand
					islandQueue = append(islandQueue, next)
				} else if grid[nextX][nextY] == water {
					grid[cur[0]][cur[1]] = boundary
				}
			}
		}
	}
}
func bfsBoundary(grid [][]int) int {
	dist := 0
	for len(islandQueue) > 0 {
		size := len(islandQueue)
		dist++
		for i := 0; i < size; i++ {
			cur := islandQueue[0]
			islandQueue = islandQueue[1:]
			for _, edge := range edges {
				nextX, nextY := cur[0]+edge[0], cur[1]+edge[1]
				next := [2]int{nextX, nextY}
				if nextX >= 0 && nextX < len(grid) && nextY >= 0 && nextY < len(grid[0]) {
					if grid[nextX][nextY] == water {
						grid[nextX][nextY] = 2
						islandQueue = append(islandQueue, next)
					} else if grid[nextX][nextY] == undiscoveredLand {
						return dist - 1
					}
				}
			}
		}
	}
	return -1
}
```

### DFS + BFS

```go
package main
```
