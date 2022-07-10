# Minimum Path Sum

## Metadata

- **Date Solved:** July 9, 2022 9:40 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-path-sum/
- **Algorithm:** dfs, dynamic programming
- **Type:** matrix

## Problem Statement

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.
Note: You can only move either down or right at any point in time.

## Problem


### Brute Force - DFS All Possible Paths (Time Limit Exceeded)

```go
package main

import "math"

var min int

func minPathSum(grid [][]int) int {
	min = math.MaxInt
	minPathSumDFS([]int{0, 0}, grid, 0)
	return min
}

func minPathSumDFS(pos []int, grid [][]int, curCount int) {

	if !posIsValid(pos, grid) {
		return
	}

	amt := grid[pos[0]][pos[1]]

	if pos[0] == len(grid)-1 && pos[1] == len(grid[0])-1 {
		if curCount+amt < min {
			min = curCount + amt
		}
		return
	}

	minPathSumDFS([]int{pos[0] + 1, pos[1]}, grid, curCount+amt)
	minPathSumDFS([]int{pos[0], pos[1] + 1}, grid, curCount+amt)
}

func posIsValid(pos []int, grid [][]int) bool {
	return pos[0] >= 0 && pos[0] < len(grid) &&
		pos[1] >= 0 && pos[1] < len(grid[0])
}
```

### Optimized - DP Tabulation

```go
package main

import "math"

var dp [200][200]int

func minPathSum(grid [][]int) int {
	dp = [200][200]int{}
	return minPathSumDFS([]int{0, 0}, grid)
}

func minPathSumDFS(pos []int, grid [][]int) int {

	if !posIsValid(pos, grid) {
		return math.MaxInt
	}

	if dp[pos[0]][pos[1]] != 0 {
		return dp[pos[0]][pos[1]]
	}

	amt := grid[pos[0]][pos[1]]

	if pos[0] == len(grid)-1 && pos[1] == len(grid[0])-1 {
		return amt
	}

	d := minPathSumDFS([]int{pos[0] + 1, pos[1]}, grid)
	r := minPathSumDFS([]int{pos[0], pos[1] + 1}, grid)

	minCount := min(d, r)
	dp[pos[0]][pos[1]] = minCount + amt
	return dp[pos[0]][pos[1]]
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}

func posIsValid(pos []int, grid [][]int) bool {
	return pos[0] >= 0 && pos[0] < len(grid) &&
		pos[1] >= 0 && pos[1] < len(grid[0])
}
```

## Solution Statement


The initial way to approach this problem is with DFS. We can traverse the matrix in DFS style, collecting the current count along the way. Whenever we reach the destination cell, we can check the current count and see if we took the minimum path. This results in TLE because we have to traverse every path in the matrix. The optimized way is to use DP with tabulation to keep track of the min path at any point in the matrix. This way we dont have to recompute that path, we just take it and move forward.
