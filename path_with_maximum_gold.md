# Path with Maximum Gold

## Metadata

- **Date Solved:** May 16, 2024 9:14 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/path-with-maximum-gold/description/?envType=daily-question&envId=2024-05-14
- **Algorithm:** backtracking, dfs, recursion
- **Type:** matrix

## Problem Statement

In a gold mine grid of size m x n, each cell in this mine has an integer representing the amount of gold in that cell, 0 if it is empty.

Return the maximum amount of gold you can collect under the conditions:

- Every time you are located in a cell you will collect all the gold in that cell.
- From your position, you can walk one step to the left, right, up, or down.
- You can't visit the same cell more than once.
- Never visit a cell with 0 gold.
- You can start and stop collecting gold from any position in the grid that has some gold.

## Constraints


- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 15
- 0 <= grid[i][j] <= 100
- There are at most 25 cells containing gold.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Limited constraints make it ok to try every path


```go
package main

var dirs = [][]int{{0, 1}, {1, 0}, {-1, 0}, {0, -1}}

func getMaximumGold(grid [][]int) int {
	M, N := len(grid), len(grid[0])
	var ans int
	var dfs func(int, int, int)
	dfs = func(row, col int, amt int) {
		curAmt := grid[row][col]
		grid[row][col] = 0
		for _, dir := range dirs {
			newRow, newCol := dir[0]+row, dir[1]+col
			if validCell(newRow, newCol, M, N) && grid[newRow][newCol] != 0 {
				dfs(newRow, newCol, curAmt+amt)
			}
		}
		grid[row][col] = curAmt
		ans = max(ans, amt+curAmt)
	}
	for i := range grid {
		for j := range grid[i] {
			if grid[i][j] != 0 {
				dfs(i, j, 0)
			}
		}
	}
	return ans
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
func validCell(row, col int, M, N int) bool {
	return row >= 0 && row < M && col >= 0 && col < N
}
```
