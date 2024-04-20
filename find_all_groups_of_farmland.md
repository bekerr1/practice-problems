# Find All Groups of Farmland

## Metadata

- **Date Solved:** April 20, 2024 12:52 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-all-groups-of-farmland/description/?envType=daily-question&envId=2024-04-20
- **Algorithm:** bfs
- **Type:** graph, matrix

## Problem Statement

You are given a 0-indexed m x n binary matrix land where a 0represents a hectare of forested land and a 1 represents a hectare of farmland.

To keep the land organized, there are designated rectangular areas of hectares that consist entirely of farmland. These rectangular areas are called groups. No two groups are adjacent, meaning farmland in one group is not four-directionally adjacent to another farmland in a different group.

land can be represented by a coordinate system where the top left corner of land is (0, 0) and the bottom right corner of land is (m-1, n-1). Find the coordinates of the top left and bottom right corner of each group of farmland. A group of farmland with a top left corner at (r1, c1) and a bottom right corner at (r2, c2) is represented by the 4-length array [r1, c1, r2, c2].

Return a 2D array containing the 4-length arrays described above for each group of farmland in land. If there are no groups of farmland, return an empty array. You may return the answer in any order.

## Constraints


- m == land.length
- n == land[i].length
- 1 <= m, n <= 300
- land consists of only 0's and 1's.
- Groups of farmland are rectangular in shape.

## Solution

- **Status:** Solved On Own


### BFS

```go
package main

var directions = [][2]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
var ans [][]int
var visited map[[2]int]bool

func findFarmland(land [][]int) [][]int {
	ans, visited = [][]int{}, make(map[[2]int]bool)
	for i := 0; i < len(land); i++ {
		for j := 0; j < len(land[i]); j++ {
			if land[i][j] == 1 && !visited[[2]int{i, j}] {
				visited[[2]int{i, j}] = true
				bfs([]int{i, j}, land)
			}
		}
	}
	return ans
}

func bfs(coord []int, land [][]int) {
	queue := [][]int{coord}
	min, max := coord, coord
	for len(queue) > 0 {
		cur := queue[0]
		max = cur
		queue = queue[1:]
		for _, dir := range directions {
			new := []int{cur[0] + dir[0], cur[1] + dir[1]}
			if valid(new, land) && !visited[[2]int{new[0], new[1]}] {
				visited[[2]int{new[0], new[1]}] = true
				queue = append(queue, new)
			}
		}
	}
	minmax := append(min, max...)
	ans = append(ans, minmax)
}

func valid(coord []int, land [][]int) bool {
	M, N := len(land), len(land[0])
	return coord[0] >= 0 && coord[0] < M && coord[1] >= 0 && coord[1] < N && land[coord[0]][coord[1]] == 1
}
```

### Greedy

```go
package main

var ans [][]int

func findFarmland(land [][]int) [][]int {
	ans = [][]int{}
	for i := 0; i < len(land); i++ {
		for j := 0; j < len(land[i]); j++ {
			if land[i][j] == 1 {
				fillAns([]int{i, j}, land)
			}
		}
	}
	return ans
}

func fillAns(coord []int, land [][]int) {
	startX, startY := coord[0], coord[1]
	var endX, endY int

	for endX = startX; endX < len(land) && land[endX][startY] == 1; endX++ {
	}
	for endY = startY; endY < len(land[0]) && land[startX][endY] == 1; endY++ {
	}

	ans = append(ans, []int{startX, startY, endX - 1, endY - 1})

	for i := startX; i < endX; i++ {
		for j := startY; j < endY; j++ {
			land[i][j] = 0
		}
	}
}
```
