# Shortest Distance from All Buildings

## Metadata

- **Date Solved:** February 2, 2024 8:46 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/shortest-distance-from-all-buildings/description/
- **Algorithm:** bfs
- **Type:** graph, matrix

## Problem Statement

You are given an m x n grid grid of values 0, 1, or 2, where:

- each 0 marks an empty land that you can pass by freely,
- each 1 marks a building that you cannot pass through, and
- each 2 marks an obstacle that you cannot pass through.

You want to build a house on an empty land that reaches all buildings in the shortest total travel distance. You can only move up, down, left, and right.

Return the shortest travel distance for such a house. If it is not possible to build such a house according to the above rules, return -1.

The total travel distance is the sum of the distances between the houses of the friends and the meeting point.
The distance is calculated using http://en.wikipedia.org/wiki/Taxicab_geometry, where distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|.

## Constraints

- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 50
- grid[i][j] is either 0, 1, or 2.
- There will be at least one building in the grid.

## Solution

- **Status:** Solved On Own


```go
package main

import "math"

var steps = [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

func shortestDistance(grid [][]int) int {
	ones := [][3]int{}
	zeroCount := 0
	for i := 0; i < len(grid); i++ {
		for j := 0; j < len(grid[0]); j++ {
			if grid[i][j] == 1 {
				ones = append(ones, [3]int{i, j, 0})
				grid[i][j] = math.MaxInt
			} else if grid[i][j] == 0 {
				zeroCount++
			} else {
				grid[i][j] = math.MaxInt - 1
			}
		}
	}
	// If we have no land to build on return -1
	if zeroCount == 0 {
		return -1
	}

	visitedBy := make(map[[2]int]int)
	for _, o := range ones {
		queue := [][3]int{o}
		visited := make(map[[2]int]bool)
		visited[[2]int{o[0], o[1]}] = true
		for len(queue) > 0 {
			qlen := len(queue)
			for i := 0; i < qlen; i++ {

				cur := queue[0]
				queue = queue[1:]
				i, j, d := cur[0], cur[1], cur[2]

				for _, st := range steps {
					newi, newj := i+st[0], j+st[1]
					if cellValid(newi, newj, grid, visited) {
						visited[[2]int{newi, newj}] = true
						visitedBy[[2]int{newi, newj}]++
						grid[newi][newj] += d + 1
						queue = append(queue, [3]int{newi, newj, d + 1})
					}
				}
			}
		}
	}

	ans := math.MaxInt
	for i := 0; i < len(grid); i++ {
		for j := 0; j < len(grid[0]); j++ {
			if visitedBy[[2]int{i, j}] == len(ones) {
				ans = min(ans, grid[i][j])
			}
		}
	}
	if ans == 0 || ans == math.MaxInt {
		return -1
	}
	return ans
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
func cellValid(i, j int, grid [][]int, visited map[[2]int]bool) bool {
	N, M := len(grid), len(grid[0])
	return i >= 0 && i < N &&
		j >= 0 && j < M &&
		!visited[[2]int{i, j}] &&
		!(grid[i][j] == math.MaxInt-1 || grid[i][j] == math.MaxInt)
}
```
