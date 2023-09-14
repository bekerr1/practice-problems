# Minimum Moves to Spread Stones Over Grid

## Metadata

- **Date Solved:** September 13, 2023 11:18 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-moves-to-spread-stones-over-grid/description/
- **Algorithm:** backtracking, dfs, dynamic programming
- **Type:** matrix

## Problem Statement

You are given a 0-indexed 2D integer matrix grid of size 3 * 3, representing the number of stones in each cell. The grid contains exactly 9 stones, and there can be multiple stones in a single cell.

In one move, you can move a single stone from its current cell to any other cell if the two cells share a side.

Return the minimum number of moves required to place one stone in each cell.

## Constraints

- grid.length == grid[i].length == 3
- 0 <= grid[i][j] <= 9
- Sum of grid is equal to 9.

## Solution

- **Status:** Needed Hints

### Solution Notes

Understanding the constraints and looking at the problem from the constraint perspective would have helped solve it.

Creating the right testcase to understand only choosing the Manhattan distance for every empty cell is not enough would have been nice.

I provided a decent solution once i got these out of the way.


```go
package main

import "math"

var emptyCells [][2]int

func minimumMoves(grid [][]int) int {
	emptyCells = [][2]int{}
	stoneCells := [][2]int{}
	for i := 0; i < 3; i++ {
		for j := 0; j < 3; j++ {
			if grid[i][j] == 0 {
				emptyCells = append(emptyCells, [2]int{i, j})
			} else if grid[i][j] > 1 {
				stoneCells = append(stoneCells, [2]int{i, j})
			}
		}
	}
	return solve(grid, stoneCells, 0)
}

func solve(grid [][]int, stoneCells [][2]int, emptyCellIdx int) int {
	if emptyCellIdx == len(emptyCells) {
		return 0
	}
	curEmpty := emptyCells[emptyCellIdx]
	var dist, ans int = 0, math.MaxInt
	for i := 0; i < len(stoneCells); i++ {
		take := stoneCells[i]
		// Only take if the grid allows it
		if grid[take[0]][take[1]] == 1 {
			continue
		}
		d := abs(curEmpty[0]-take[0]) + abs(curEmpty[1]-take[1])
		grid[take[0]][take[1]]--
		dist = d + solve(grid, stoneCells, emptyCellIdx+1)
		grid[take[0]][take[1]]++
		ans = min(dist, ans)
	}
	return ans
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```
