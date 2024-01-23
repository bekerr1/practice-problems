# Minimum Falling Path Sum

## Metadata

- **Date Solved:** January 22, 2024 9:07 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-falling-path-sum/description/?envType=daily-question&envId=2024-01-19
- **Algorithm:** dynamic programming
- **Type:** matrix

## Problem Statement

Given an n x n array of integers matrix, return the minimum sum of any falling path through matrix.

A falling path starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position (row, col) will be (row + 1, col - 1), (row + 1, col), or (row + 1, col + 1).

## Constraints

- n == matrix.length == matrix[i].length
- 1 <= n <= 100
- -100 <= matrix[i][j] <= 100

## Solution

- **Status:** Solved On Own


```go
package main

import "math"

func minFallingPathSum(matrix [][]int) int {
	N, M := len(matrix), len(matrix[0])
	prevRow := make([]int, len(matrix))
	row := make([]int, len(matrix))
	for i := 0; i < N; i++ {
		row = make([]int, len(matrix))
		for j := 0; j < M; j++ {
			row[j] = matrix[i][j] + prevRow[j]
			if j == 0 && M > 1 {
				row[j] = min(matrix[i][j]+prevRow[j], matrix[i][j]+prevRow[j+1])
			} else if M > 1 && j == len(matrix[i])-1 {
				row[j] = min(matrix[i][j]+prevRow[j-1], matrix[i][j]+prevRow[j])
			} else if M > 2 {
				row[j] = min(matrix[i][j]+prevRow[j-1],
					min(matrix[i][j]+prevRow[j], matrix[i][j]+prevRow[j+1]))
			}
		}
		prevRow = row
	}
	ans := math.MaxInt
	for _, n := range row {
		ans = min(ans, n)
	}
	return ans
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
