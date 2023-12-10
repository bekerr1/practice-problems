# Maximum Strictly Increasing Cells in a Matrix

## Metadata

- **Date Solved:** December 10, 2023 2:09 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/maximum-strictly-increasing-cells-in-a-matrix/description/
- **Algorithm:** dynamic programming
- **Type:** matrix

## Problem Statement

Given a 1-indexed m x n integer matrix mat, you can select any cell in the matrix as your starting cell.
From the starting cell, you can move to any other cell in the same row or column, but only if the value of the destination cell is strictly greater than the value of the current cell. You can repeat this process as many times as possible, moving from cell to cell until you can no longer make any moves.

Your task is to find the maximum number of cells that you can visit in the matrix by starting from some cell.

Return an integer denoting the maximum number of cells that can be visited.

## Constraints

- m == mat.length 
- n == mat[i].length 
- 1 <= m, n <= 105
- 1 <= m * n <= 105
- -105 <= mat[i][j] <= 105

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Duplicate values are what got me here. A really good explanation on how to approach the problem: https://www.youtube.com/watch?v=Chl2-fBuSOs

I was able to come up with the top-down recursive solution but that was to large of time complexity O((n+m)*(n+m)).

Sorting by value here and iterating through the values in order lessened the complexity to O(nmlog(nm)), which was able to pass


### Optimized

```go
package main

import "sort"

func maxIncreasingCells(mat [][]int) int {
	flatMat := make(map[int][][2]int)
	keys := []int{}
	for i := 0; i < len(mat); i++ {
		for j := 0; j < len(mat[i]); j++ {
			var entries [][2]int
			var exists bool
			if entries, exists = flatMat[mat[i][j]]; !exists {
				entries = make([][2]int, 0)
				keys = append(keys, mat[i][j])
			}
			entries = append(entries, [2]int{i, j})
			flatMat[mat[i][j]] = entries
		}
	}

	sort.Slice(keys, func(i, j int) bool {
		return keys[i] > keys[j]
	})

	maxRows := make([]int, len(mat))
	maxCols := make([]int, len(mat[0]))
	result := 1
	for _, key := range keys {

		curMaxRows := make(map[int]int, len(flatMat[key]))
		curMaxCols := make(map[int]int, len(flatMat[key]))

		for _, cell := range flatMat[key] {
			row, col := cell[0], cell[1]

			val := 1 + max(maxRows[row], maxCols[col])
			result = max(result, val)

			curMaxRows[row] = max(curMaxRows[row], val)
			curMaxCols[col] = max(curMaxCols[col], val)
		}

		for r, v := range curMaxRows {
			maxRows[r] = max(maxRows[r], v)
		}
		for c, v := range curMaxCols {
			maxCols[c] = max(maxCols[c], v)
		}
	}
	return result
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Initial DP

```go
package main

import "fmt"

var dp map[[2]int]int

func maxIncreasingCells(mat [][]int) int {
	dp = make(map[[2]int]int)
	ans := 0
	for i := 0; i < len(mat); i++ {
		for j := 0; j < len(mat[i]); j++ {
			fmt.Println("solving: ", i, j)
			ans = max(ans, solve(mat, i, j))
		}
	}
	return ans
}

func solve(mat [][]int, curX, curY int) int {
	if v, exists := dp[[2]int{curX, curY}]; exists {
		return v
	}

	curVal := mat[curX][curY]
	ansCol, ansRow := 1, 1

	// Iterate the cols
	for i := 0; i < len(mat[0]); i++ {
		if i == curY {
			continue
		}
		if mat[curX][i] > curVal {
			ansCol = max(ansCol, 1+solve(mat, curX, i))
		}
	}

	// Iterate the rows
	for i := 0; i < len(mat); i++ {
		if i == curX {
			continue
		}
		if mat[i][curY] > curVal {
			ansRow = max(ansRow, 1+solve(mat, i, curY))
		}
	}
	dp[[2]int{curX, curY}] = max(ansCol, ansRow)
	return dp[[2]int{curX, curY}]
}
```
