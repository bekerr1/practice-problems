# Maximum Number of Points with Cost

## Metadata

- **Date Solved:** August 17, 2022 3:19 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-number-of-points-with-cost/
- **Algorithm:** dynamic programming, matrix
- **Type:** graph, precomputations, weighted path

## Problem Statement

You are given an m x n integer matrix points (0-indexed). Starting with 0 points, you want to maximize the number of points you can get from the matrix.
To gain points, you must pick one cell in each row. Picking the cell at coordinates (r, c) will add points[r][c] to your score.
However, you will lose points if you pick a cell too far from the cell that you picked in the previous row. For every two adjacent rows r and r + 1 (where 0 <= r < m - 1), picking cells at coordinates (r, c1) and (r + 1, c2) will subtract abs(c1 - c2) from your score.
Return the maximum number of points you can achieve.
abs(x) is defined as:
- x for x >= 0.
- -x for x < 0.

```go
package main

func maxPoints(points [][]int) int64 {
	dp := make([]int64, len(points[0]))

	for j := range dp {
		dp[j] = points[0][j]
	}

	col := len(points[0])
	leftDP := make([]int64, col)
	rightDP := make([]int64, col)
	for i := 1; i < len(points); i++ {

		leftDP[0] = dp[0]
		rightDP[col-1] = dp[col-1]
		for j := 1; j < col; j++ {
			leftDP[j] = max(leftDP[j-1]-1, dp[j])
		}
		for j := col - 2; j <= 0; j-- {
			rightDP[j] = max(leftDP[j+1]-1, dp[j])
		}

		for j := 0; j < col; j++ {
			dp[j] = points[i][j] + max(leftDP[j], rightDP[j])
		}
	}
	var max int64
	for _, n := range dp {
		if n > max {
			max = n
		}
	}
	return max
}

func max(x, y int64) int64 {
	if x > y {
		return x
	}
	return y
}
```

### Testcases

[[1, 2, 3], [3, 2, 1], [1, 1, 4]]

[[1, 1, 1, 1, 2], [1, 3, 1, 0, 0], [1, 1, 3, 1, 4]]

[[1, 1, 1], [1, 1, 1], [1, 1, 1]]

[[]]

## Solution


```go
package main

func maxPoints(points [][]int) int64 {
	var (
		maxVal       int64
		col          = len(points[0])
		dp           = make([]int64, len(points[0]))
		horizontalDP = make([]int64, col)
	)

	for j := range dp {
		dp[j] = int64(points[0][j])
		maxVal = max(maxVal, dp[j])
	}

	for i := 1; i < len(points); i++ {

		horizontalDP[0] = dp[0]
		for j := 1; j < col; j++ {
			horizontalDP[j] = max(horizontalDP[j-1]-1, dp[j])
		}

		horizontalDP[col-1] = max(horizontalDP[col-1], dp[col-1])
		for j := col - 2; j >= 0; j-- {
			horizontalDP[j] = max(horizontalDP[j], max(horizontalDP[j+1]-1, dp[j]))
		}

		for j := 0; j < col; j++ {
			dp[j] = int64(points[i][j]) + horizontalDP[j]
			maxVal = max(maxVal, dp[j])
		}
	}
	return maxVal
}

func max(x, y int64) int64 {
	if x > y {
		return x
	}
	return y
}
```

### Problem Statements

This problem was very tricky but rewarding when you understand it. There is essentially pre-computations that need to be performed on all the rows from left and right to verify the column has the current max value for the row. The brute force way to look at this is, in a row one would have to, for each column, iterate through every column in the previous row and see what the max value would be given the constraints on distance between the current row and previous. Instead of doing this for every column, we keep a horizontal DP array and keep a row-wise base case of points[i][0]. Then from there we either take the max of the previous rows column points[i-1][j] or  the previous column at this row points[i][j-1]. If we do this from the left and the right, we can end up with a collection that has to max values for that row at each column index.
