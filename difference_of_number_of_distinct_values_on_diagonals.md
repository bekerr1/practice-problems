# Difference of Number of Distinct Values on Diagonals

## Metadata

- **Date Solved:** June 3, 2023 6:04 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/difference-of-number-of-distinct-values-on-diagonals/description/
- **Algorithm:** brute force, hashmap
- **Type:** array, matrix

## Problem Statement

Given a 0-indexed 2D grid of size m x n, you should find the matrix answer of size m x n.
The value of each cell (r, c) of the matrix answer is calculated in the following way:
- Let topLeft[r][c] be the number of distinct values in the top-left diagonal of the cell (r, c) in the matrix grid.
- Let bottomRight[r][c] be the number of distinct values in the bottom-right diagonal of the cell (r, c) in the matrix grid.
Then answer[r][c] = |topLeft[r][c] - bottomRight[r][c]|.
Return the matrix answer.
A matrix diagonal is a diagonal line of cells starting from some cell in either the topmost row or leftmost column and going in the bottom-right direction until reaching the matrix's end.
A cell (r1, c1) belongs to the top-left diagonal of the cell (r, c), if both belong to the same diagonal and r1 < r. Similarly is defined bottom-right diagonal.

## Constraints

- m == grid.length
- n == grid[i].length
- 1 <= m, n, grid[i][j] <= 50
## Solution

### Solution Notes

I tried to calculate the distinct values per cell going down and then attempting to subtract to get sections of matrix. This was a bad idea. Subtracting out values in this way does not account for distinct values within the current section.



```go
package main

func differenceOfDistinctValues(grid [][]int) [][]int {

	ret := make([][]int, len(grid))
	for i := 0; i < len(grid); i++ {
		ret[i] = make([]int, len(grid[i]))
		for j := 0; j < len(grid[i]); j++ {
			tl := topLeft(grid, i, j)
			br := bottomRight(grid, i, j)
			ret[i][j] = abs(tl - br)
		}
	}
	return ret
}

func topLeft(grid [][]int, i, j int) int {
	dv := make(map[int]bool)
	for i, j := i-1, j-1; i >= 0 && j >= 0; i, j = i-1, j-1 {
		dv[grid[i][j]] = true
	}
	return len(dv)
}

func bottomRight(grid [][]int, i, j int) int {
	dv := make(map[int]bool)
	for i, j := i+1, j+1; i < len(grid) && j < len(grid[i]); i, j = i+1, j+1 {
		dv[grid[i][j]] = true
	}
	return len(dv)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```
