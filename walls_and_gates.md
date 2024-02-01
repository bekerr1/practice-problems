# Walls and Gates

## Metadata

- **Date Solved:** January 31, 2024 8:02 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/walls-and-gates/description/
- **Algorithm:** bfs
- **Type:** graph

## Problem Statement

You are given an m x n grid rooms initialized with these three possible values.

- -1 A wall or an obstacle.
- 0 A gate.
- INF Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

## Constraints

- m == rooms.length
- n == rooms[i].length
- 1 <= m, n <= 250
- rooms[i][j] is -1, 0, or 231 - 1.

## Solution

- **Status:** Solved On Own


```go
package main

var steps = [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

func wallsAndGates(rooms [][]int) {

	queue := [][2]int{}
	for i := 0; i < len(rooms); i++ {
		for j := 0; j < len(rooms[0]); j++ {
			if rooms[i][j] == 0 {
				queue = append(queue, [2]int{i, j})
			}
		}
	}

	for len(queue) > 0 {
		cur := queue[0]
		queue = queue[1:]
		curi, curj := cur[0], cur[1]
		dist := rooms[curi][curj]
		for _, st := range steps {
			newi, newj := curi+st[0], curj+st[1]
			if cellValid(newi, newj, rooms) &&
				rooms[newi][newj] > dist+1 {
				rooms[newi][newj] = dist + 1
				queue = append(queue, [2]int{newi, newj})
			}
		}
	}
}

func cellValid(i, j int, rooms [][]int) bool {
	N, M := len(rooms), len(rooms[0])
	return i >= 0 && i < N && j >= 0 && j < M
}
```
