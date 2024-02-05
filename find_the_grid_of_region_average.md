# Find the Grid of Region Average

## Metadata

- **Date Solved:** February 4, 2024 10:42 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-grid-of-region-average/description/
- **Algorithm:** brute force
- **Type:** math, matrix

## Problem Statement

You are given a 0-indexed m x n grid image which represents a grayscale image, where image[i][j] represents a pixel with intensity in the range[0..255]. You are also given a non-negative integer threshold.
Two pixels image[a][b] and image[c][d] are said to be adjacent if |a - c| + |b - d| == 1.

A region is a 3 x 3 subgrid where the absolute difference in intensity between any two adjacent pixels is less than or equal to threshold.

All pixels in a region belong to that region, note that a pixel can belong to multipleregions.

You need to calculate a 0-indexed m x n grid result, where result[i][j] is the average intensity of the region to which image[i][j] belongs, rounded down to the nearest integer. If image[i][j] belongs to multiple regions, result[i][j] is the average of the rounded down average intensities of these regions, rounded downto the nearest integer. If image[i][j] does not belong to any region, result[i][j]is equal to image[i][j].

Return the grid result.

## Constraints

- 3 <= n, m <= 500
- 0 <= image[i][j] <= 255
- 0 <= threshold <= 255

## Solution

- **Status:** Needed Hints


```go
package main

func resultGrid(image [][]int, threshold int) [][]int {
	N, M := len(image), len(image[0])
	sum := make([][]int, N)
	num := make([][]int, N)
	for i := range sum {
		sum[i] = make([]int, M)
		num[i] = make([]int, M)
	}

	for i := 0; i < N; i++ {
		for j := 0; j < M; j++ {
			traverseRegion(i, j, N, M, threshold, sum, num, image)
		}
	}

	res := make([][]int, N)
	for i := 0; i < N; i++ {
		res[i] = make([]int, M)
		for j := 0; j < M; j++ {
			v := image[i][j]
			if num[i][j] > 0 {
				v = sum[i][j] / num[i][j]
			}
			res[i][j] = v
		}
	}
	return res
}

var xDir = []int{0, 1}
var yDir = []int{-1, 0}

func traverseRegion(row, col, N, M, t int, sum, num [][]int, image [][]int) {
	avg := 0
	for i := row; row+3 <= N && i < row+3; i++ {
		for j := col; col+3 <= M && j < col+3; j++ {
			for d := 0; d < 2; d++ {
				adji, adjj := i+xDir[d], j+yDir[d]
				if adji >= row && adji < row+3 && adjj >= col && adjj < col+3 &&
					abs(image[i][j]-image[adji][adjj]) > t {
					return
				}
			}
			avg += image[i][j]
		}
	}
	for i := row; row+3 <= N && i < row+3; i++ {
		for j := col; col+3 <= M && j < col+3; j++ {
			sum[i][j] = sum[i][j] + (avg / 9)
			num[i][j]++
		}
	}
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```
