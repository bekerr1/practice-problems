# 01 Matrix

## Metadata

- **Date Solved:** January 18, 2024 3:55 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/01-matrix/description/
- **Algorithm:** bfs, dynamic programming
- **Type:** matrix

## Problem Statement

Given an m x n binary matrix mat, return the distance of the nearest 0for each cell.

The distance between two adjacent cells is 1.

## Constraints

- m == mat.length
- n == mat[i].length
- 1 <= m, n <= 104
- 1 <= m * n <= 104
- mat[i][j] is either 0 or 1.
- There is at least one 0 in mat.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I was able to eventually come up with a solution that cleared 49/50 test cases but was an O(n^2*m^2) solution so it was bad in the worst case. The approach I took was to do BFS on a cell every time we encountered a '1' and cache the result for that cell (to possibly be used later). If all the '1's were filled in the array except for the last cell, this algorithm performs in the worst case (test case 50).

The correct solution here was to queue all cells at 0 and keep track of the steps from this cell to others. We could then, as we encounter cells, mark the min steps from any cell to this one. In this way, we would have a runtime of O(n*m) as this is what BFS would perform in.

Again this is another problem where I did the opposite of what I should have done. I need to try to think differently to solve some problems. Instead of start from 1 I start from 0 and do BFS in the correct runtime. Ultimately maintaining the runtime of the algorithm should be key.


```go
package main

import "math"

var directions = [][2]int{{0, 1}, {1, 0}, {-1, 0}, {0, -1}}

func updateMatrix(mat [][]int) [][]int {
	N, M := len(mat), len(mat[0])
	seen := make(map[[2]int]bool)
	queue := [][3]int{}
	for i := 0; i < N; i++ {
		for j := 0; j < M; j++ {
			if mat[i][j] == 0 {
				queue = append(queue, [3]int{i, j, 0})
				seen[[2]int{i, j}] = true
			}
		}
	}

	for len(queue) > 0 {
		cur := queue[0]
		queue = queue[1:]
		i, j, steps := cur[0], cur[1], cur[2]
		mat[i][j] = min(mat[i][j], steps)
		for _, d := range directions {
			newi, newj := i+d[0], j+d[1]
			if seen[[2]int{newi, newj}] ||
				!validCell(newi, newj, N, M) {
				continue
			}

			queue = append(queue, [3]int{newi, newj, steps + 1})
			seen[[2]int{newi, newj}] = true
			mat[newi][newj] = math.MaxInt
		}
	}
	return mat
}
func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
func validCell(i, j int, N, M int) bool { return i >= 0 && i < N && j >= 0 && j < M }
```
