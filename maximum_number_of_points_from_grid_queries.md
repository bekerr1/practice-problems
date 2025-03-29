# Maximum Number of Points From Grid Queries

## Metadata

- **Date Solved:** March 28, 2025 10:41 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/maximum-number-of-points-from-grid-queries/description/?envType=daily-question&envId=2025-03-28
- **Algorithm:** bfs, graph, heap
- **Type:** matrix

## Problem Statement

You are given an m x n integer matrix grid and an array queries of size k.
Find an array answer of size k such that for each integer queries[i] you start in the top left cell of the matrix and repeat the following process:

- If queries[i] is strictly greater
than the value of the current cell that you are in, then you get one
point if it is your first time visiting this cell, and you can move to

## Constraints


- m == grid.length
- n == grid[i].length
- 2 <= m, n <= 1000
- 4 <= m * n <= 105
- k == queries.length
- 1 <= k <= 104
- 1 <= grid[i][j], queries[i] <= 106

## Solution

- **Status:** Solved On Own

### Solution Notes

Analyzing the obvious solutions time complexity and pivoting to something more optimal was key here.


```go
package main

import (
	"container/heap"
	"sort"
)

var dirs = [4][2]int{{0, 1}, {-1, 0}, {0, -1}, {1, 0}}

func maxPoints(grid [][]int, queries []int) []int {
	M, N := len(grid), len(grid[0])
	queryAndIndex := [][2]int{}
	for i, q := range queries {
		queryAndIndex = append(queryAndIndex, [2]int{i, q})
	}
	// Sort by query value. We maintain idx so we can build answers
	sort.Slice(queryAndIndex, func(i, j int) bool {
		return queryAndIndex[i][1] < queryAndIndex[j][1]
	})
	// [[2 2], [0 5], [1 6]]
	ans := make([]int, len(queries))

	minHeap := IntHeap{[3]int{grid[0][0], 0, 0}}
	heap.Init(&minHeap)
	for queryCount, queryIndex := range queryAndIndex {
		idx, query := queryIndex[0], queryIndex[1] // 2 2 | 0 5
		if minHeap.Len() == 0 {
			ans[idx] = M * N
			continue
		} else if queryCount > 0 {
			// set the current ans to the count of the prev query
			// It is >= the last value so will have atleast that many
			ans[idx] = ans[queryAndIndex[queryCount-1][0]]
		}

		for minHeap.Len() > 0 {
			// peek at the min value to see if we can account it
			if query > minHeap[0][0] {
				cur := heap.Pop(&minHeap).([3]int)
				i, j := cur[1], cur[2]
				if grid[i][j] == 0 {
					continue
				}
				grid[i][j] = 0
				ans[idx]++
				for _, d := range dirs {
					newi, newj := i+d[0], j+d[1]
					// As long as the value is valid and not visited, push it onto the heap
					if valid(newi, newj, M, N) && grid[newi][newj] > 0 {
						heap.Push(&minHeap, [3]int{grid[newi][newj], newi, newj})
					}
				}
			} else {
				break
			}
		}
	}
	return ans
}

func valid(i, j, M, N int) bool {
	return i >= 0 && i < M && j >= 0 && j < N
}

// An IntHeap is a min-heap of ints.
type IntHeap [][3]int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i][0] < h[j][0] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x any) {
	// Push and Pop use pointer receivers because they modify the slice's length,
	// not just its contents.
	*h = append(*h, x.([3]int))
}

func (h *IntHeap) Pop() any {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}
```
