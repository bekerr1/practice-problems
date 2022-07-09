# Cherry Pickup

## Metadata

- **Date Solved:** July 9, 2022 12:44 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/cherry-pickup/
- **Algorithm:** dfs, dynamic programming
- **Type:** matrix

## Problem Statement

You are given an n x n grid representing a field of cherries, each cell is one of three possible integers.
- 0 means the cell is empty, so you can pass through,
- 1 means the cell contains a cherry that you can pick up and pass through, or
- -1 means the cell contains a thorn that blocks your way.
Return the maximum number of cherries you can collect by following the rules below:
- Starting at the position (0, 0) and reaching (n - 1, n - 1) by moving right or down through valid path cells (cells with value 0 or 1).
- After reaching (n - 1, n - 1), returning to (0, 0) by moving left or up through valid path cells.
- When passing through a path cell containing a cherry, you pick it up, and the cell becomes an empty cell 0.
- If there is no valid path between (0, 0) and (n - 1, n - 1), then no cherries can be collected.

## Problem


### Brute Force (Time Limit Exceeded)

```go
package main

var downLeft = [][]int{{1, 0}, {0, 1}}
var upRight = [][]int{{-1, 0}, {0, -1}}

var src = []int{0, 0}
var dst []int
var cherryCount int

func cherryPickup(grid [][]int) int {
	cherryCount = 0
	dst = []int{len(grid) - 1, len(grid[0]) - 1}
	cherryPickupDFS(src, dst, grid, downLeft, 0, false)
	return cherryCount
}

func cherryPickupDFS(pos, end []int, grid, movement [][]int, count int, completed bool) {
	if !(pos[0] >= 0 && pos[0] < len(grid) &&
		pos[1] >= 0 && pos[1] < len(grid[0])) ||
		grid[pos[0]][pos[1]] == -1 {
		return
	}
	if pos[0] == end[0] && pos[1] == end[1] &&
		end[0] == src[0] && end[1] == src[1] &&
		completed {
		cherryCount = max(cherryCount, count)
		return
	}
	amt := grid[pos[0]][pos[1]]
	grid[pos[0]][pos[1]] = 0
	for _, move := range movement {
		if pos[0] == end[0] && pos[1] == end[1] && !completed {
			cherryPickupDFS(pos, src, grid, upRight, count+amt, true)
		} else {
			cherryPickupDFS([]int{pos[0] + move[0], pos[1] + move[1]}, end,
				grid, movement, count+amt, completed)
		}
	}
	grid[pos[0]][pos[1]] = amt
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Optimized - Two Positions (Time Limit Exceeded)

```go
package main

import "math"

var movement = [4][2]int{{1, 0}, {0, 1}, {-1, 0}, {0, -1}}

func cherryPickup(grid [][]int) int {
	src := [2]int{0, 0}
	dst := [2]int{len(grid) - 1, len(grid[0]) - 1}
	// cherryPickupDFS will return negative for an incomplete
	// DFS
	return max(0, cherryPickupDFS(src, src, dst, grid, 0))
}

func cherryPickupDFS(pos1, pos2, end [2]int, grid [][]int, count int) int {
	if !posInGrid(pos1, grid) || !posInGrid(pos2, grid) ||
		grid[pos1[0]][pos1[1]] == -1 || grid[pos2[0]][pos2[1]] == -1 {
		return math.MinInt // cancel out
	}

	if pos1[0] == end[0] && pos1[1] == end[1] {
		return grid[pos1[0]][pos1[1]]
	}
	if pos2[0] == end[0] && pos2[1] == end[1] {
		return grid[pos2[0]][pos2[1]]
	}

	var amt int
	if pos1[0] == pos2[0] && pos1[1] == pos2[1] {
		amt = grid[pos1[0]][pos1[1]]
	} else {
		amt = grid[pos1[0]][pos1[1]] + grid[pos2[0]][pos2[1]]
	}

	dd := cherryPickupDFS([2]int{pos1[0] + 1, pos1[1] + 0}, [2]int{pos2[0] + 1, pos2[1] + 0},
		end, grid, count+amt)
	dr := cherryPickupDFS([2]int{pos1[0] + 1, pos1[1] + 0}, [2]int{pos2[0] + 0, pos2[1] + 1},
		end, grid, count+amt)
	rd := cherryPickupDFS([2]int{pos1[0] + 0, pos1[1] + 1}, [2]int{pos2[0] + 1, pos2[1] + 0},
		end, grid, count+amt)
	rr := cherryPickupDFS([2]int{pos1[0] + 0, pos1[1] + 1}, [2]int{pos2[0] + 0, pos2[1] + 1},
		end, grid, count+amt)
	maxCherries := max(max(dd, dr), max(rd, rr))
	return amt + maxCherries
}

func posInGrid(pos [2]int, grid [][]int) bool {
	return pos[0] >= 0 && pos[0] < len(grid) &&
		pos[1] >= 0 && pos[1] < len(grid[0])
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Optimized - DP Tabulation (Accepted)

```go
package main

import "math"

var dp [50][50][50][50]int

func cherryPickup(grid [][]int) int {
	src := [2]int{0, 0}
	dst := [2]int{len(grid) - 1, len(grid[0]) - 1}
	dp = [50][50][50][50]int{}
	// cherryPickupDFS will return negative for an incomplete
	// DFS
	return max(0, cherryPickupDFS(src, src, dst, grid, 0))
}

func cherryPickupDFS(pos1, pos2, end [2]int, grid [][]int, count int) int {
	if !posInGrid(pos1, grid) || !posInGrid(pos2, grid) ||
		grid[pos1[0]][pos1[1]] == -1 || grid[pos2[0]][pos2[1]] == -1 {
		return math.MinInt // cancel out
	}

	if dp[pos1[0]][pos1[1]][pos2[0]][pos2[1]] != 0 {
		return dp[pos1[0]][pos1[1]][pos2[0]][pos2[1]]
	}

	if pos1[0] == end[0] && pos1[1] == end[1] {
		return grid[pos1[0]][pos1[1]]
	}
	if pos2[0] == end[0] && pos2[1] == end[1] {
		return grid[pos2[0]][pos2[1]]
	}

	var amt int
	if pos1[0] == pos2[0] && pos1[1] == pos2[1] {
		amt = grid[pos1[0]][pos1[1]]
	} else {
		amt = grid[pos1[0]][pos1[1]] + grid[pos2[0]][pos2[1]]
	}

	dd := cherryPickupDFS([2]int{pos1[0] + 1, pos1[1] + 0}, [2]int{pos2[0] + 1, pos2[1] + 0},
		end, grid, count+amt)
	dr := cherryPickupDFS([2]int{pos1[0] + 1, pos1[1] + 0}, [2]int{pos2[0] + 0, pos2[1] + 1},
		end, grid, count+amt)
	rd := cherryPickupDFS([2]int{pos1[0] + 0, pos1[1] + 1}, [2]int{pos2[0] + 1, pos2[1] + 0},
		end, grid, count+amt)
	rr := cherryPickupDFS([2]int{pos1[0] + 0, pos1[1] + 1}, [2]int{pos2[0] + 0, pos2[1] + 1},
		end, grid, count+amt)

	amt += max(max(dd, dr), max(rd, rr))

	dp[pos1[0]][pos1[1]][pos2[0]][pos2[1]] = amt
	return dp[pos1[0]][pos1[1]][pos2[0]][pos2[1]]
}

func posInGrid(pos [2]int, grid [][]int) bool {
	return pos[0] >= 0 && pos[0] < len(grid) &&
		pos[1] >= 0 && pos[1] < len(grid[0])
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

## Solution Statement

