# Find the Safest Path in a Grid

## Metadata

- **Date Solved:** August 20, 2023 2:11 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-safest-path-in-a-grid/description/
- **Algorithm:** bfs, binary search, dijkstra
- **Type:** graph, matrix

## Problem Statement

You are given a 0-indexed 2D matrix grid of size n x n, where (r, c) represents:

- A cell containing a thief if grid[r][c] = 1
- An empty cell if grid[r][c] = 0

You are initially positioned at cell (0, 0). In one move, you can move to any adjacent cell in the grid, including cells containing thieves.

The safeness factor of a path on the grid is defined as the minimum manhattan distance from any cell in the path to any thief in the grid.

Return the maximum safeness factor of all paths leading to cell (n - 1, n - 1).

An adjacent cell of cell (r, c), is one of the cells (r, c + 1), (r, c - 1), (r + 1, c) and (r - 1, c) if it exists.

The Manhattan distance between two cells (a, b) and (x, y) is equal to |a - x| + |b - y|, where |val| denotes the absolute value of val.

## Constraints

- 1 <= grid.length == n <= 400
- grid[i].length == n
- grid[i][j] is either 0 or 1.
- There is at least one thief in the grid.

## Solution

- **Status:** Needed Hints

### Solution Notes

Had the idea to fill the grid with pre-calculated safeness factor but came up with the wrong approach initially. Needed a hint on exactly how to do it. Additionally, though of dijkstra’s to follow the max safeness so that is good.

Implementation is long, but other approaches seemed long also. Overall had the right ideas, just could not execute on my own. Had a few corner cases that i needed to handle through trial and error when submitting also.


```go
package main

import (
	"container/heap"
	"math"
)

type PriorityQueue [][2]int

func (h PriorityQueue) Len() int      { return len(h) }
func (h PriorityQueue) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h PriorityQueue) Less(i, j int) bool {
	g1, g2 := h[i], h[j]
	return cgrid[g1[0]][g1[1]] > cgrid[g2[0]][g2[1]]
}
func (h *PriorityQueue) Push(x interface{}) {
	*h = append(*h, x.([2]int))
}
func (h *PriorityQueue) Pop() interface{} {
	tmp := *h
	x := tmp[len(tmp)-1]
	*h = tmp[0 : len(tmp)-1]
	return x
}

var cgrid [][]int
var start [2]int
var end [2]int

func maximumSafenessFactor(grid [][]int) int {
	// If a thief is at start or end, return 0
	start = [2]int{0, 0}
	end = [2]int{len(grid) - 1, len(grid[0]) - 1}
	if grid[0][0] == 1 || grid[len(grid)-1][len(grid[0])-1] == 1 {
		return 0
	}

	cgrid = grid
	queue := PriorityQueue{}
	visited = make(map[[2]int]struct{})
	for i := 0; i < len(grid); i++ {
		for j := 0; j < len(grid[i]); j++ {
			if grid[i][j] == 1 {
				grid[i][j] = 0
				theif := [2]int{i, j}
				queue = append(queue, theif)
				visited[theif] = struct{}{}
			}
		}
	}
	// BFS to calculate the safeness factor for all spaces
	fillGridSafenessFactor(grid, queue)
	// Run dijkstra against the grid with the shortest path
	// defined as following the grid with the max safeness factor.
	// We will keep the min value encountered and return it.
	visited = make(map[[2]int]struct{})
	queue = PriorityQueue{}
	return dijkstra(grid, queue)
}

var diffxy = [4][2]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
var visited map[[2]int]struct{}

func dijkstra(grid [][]int, pq PriorityQueue) int {
	// safeness will be the min number
	// we encounter traversing in dijkstra's
	safeness := math.MaxInt

	pq = append(pq, [2]int{0, 0})
	heap.Init(&pq)
	for pq.Len() > 0 {
		cur := heap.Pop(&pq).([2]int)
		safeness = min(safeness, grid[cur[0]][cur[1]])
		if cur == end {
			return safeness
		}
		for _, dxy := range diffxy {
			n := [2]int{cur[0] + dxy[0], cur[1] + dxy[1]}
			_, exists := visited[n]
			if neighborValid(grid, n) && !exists {
				heap.Push(&pq, n)
				visited[n] = struct{}{}
			}
		}
	}
	return safeness
}

func fillGridSafenessFactor(grid [][]int, queue [][2]int) {
	for len(queue) > 0 {
		cur := queue[0]
		queue = queue[1:]
		visited[cur] = struct{}{}

		for _, dxy := range diffxy {
			n := [2]int{cur[0] + dxy[0], cur[1] + dxy[1]}
			if neighborValid(grid, n) {
				_, exists := visited[n]
				if exists {
					grid[n[0]][n[1]] = min(grid[cur[0]][cur[1]]+1, grid[n[0]][n[1]])
				} else {
					grid[n[0]][n[1]] = grid[cur[0]][cur[1]] + 1
					visited[n] = struct{}{}
					queue = append(queue, n)
				}
			}
		}
	}
}

func neighborValid(grid [][]int, n [2]int) bool {
	return n[0] >= 0 && n[0] < len(grid) && n[1] >= 0 && n[1] < len(grid[0])
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
