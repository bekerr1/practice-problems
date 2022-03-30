# Search a 2D Matrix

## Metadata

- **Date Solved:** March 29, 2022 10:14 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/search-a-2d-matrix/
- **Algorithm:** binary search
- **Type:** array

## Problem Statement

Write an efficient algorithm that searches for a value target in an m x n integer matrix matrix. This matrix has the following properties:
- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

```go
package main

import "fmt"

func searchMatrix(matrix [][]int, target int) bool {
	var (
		row = 0
		col = 0
		mid = 0

		rowHigh = len(matrix) - 1
		colHigh = len(matrix[0]) - 1
	)

	for row < rowHigh {
		mid = (row + rowHigh) / 2
		fmt.Println(mid, row, rowHigh)
		if matrix[mid][colHigh] == target {
			return true
		} else if target < matrix[mid][colHigh] {
			rowHigh = mid
		} else {
			row = mid + 1
		}
	}

	for col <= colHigh {
		mid = (col + colHigh) / 2
		if matrix[rowHigh][mid] == target {
			return true
		} else if target < matrix[rowHigh][mid] {
			colHigh = mid - 1
		} else {
			col = mid + 1
		}
	}
	return false
}
```

## Solution Notes

