# Leftmost Column with at Least a One

## Metadata

- **Date Solved:** May 9, 2024 10:17 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/leftmost-column-with-at-least-a-one/
- **Algorithm:** binary search
- **Type:** matrix

## Problem Statement

A row-sorted binary matrix means that all elements are 0 or 1 and each row of the matrix is sorted in non-decreasing order.

Given a row-sorted binary matrix binaryMatrix, return the index (0-indexed) of the leftmost column with a 1 in it. If such an index does not exist, return -1.

You can't access the Binary Matrix directly. You may only access the matrix using a BinaryMatrix interface:

- BinaryMatrix.get(row, col) returns the element of the matrix at index (row, col) (0-indexed).
- BinaryMatrix.dimensions() returns the dimensions of the matrix as a list of 2 elements [rows, cols], which means the matrix is rows x cols.

Submissions making more than 1000 calls to BinaryMatrix.get will be judged Wrong Answer. Also, any solutions that attempt to circumvent the judge will result in disqualification.

For custom testing purposes, the input will be the entire binary matrix mat. You will not have access to the binary matrix directly.

## Constraints


- rows == mat.length
- cols == mat[i].length
- 1 <= rows, cols <= 100
- mat[i][j] is either 0 or 1.
- mat[i] is sorted in non-decreasing order.

## Solution

- **Status:** Solved On Own


```go
package main

import "math"

func leftMostColumnWithOne(binaryMatrix BinaryMatrix) int {
	dim := binaryMatrix.Dimensions()
	rows, cols := dim[0], dim[1]

	ans := math.MaxInt
	for i := 0; i < rows; i++ {
		low, high := 0, cols-1
		for low <= high {
			mid := low + (high-low)/2
			val := binaryMatrix.Get(i, mid)
			if val == 1 {
				high = mid - 1
				ans = min(ans, mid)
			} else {
				low = mid + 1
			}
		}
	}
	if ans == math.MaxInt {
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
```
