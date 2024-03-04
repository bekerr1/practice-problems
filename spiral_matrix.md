# Spiral Matrix

## Metadata

- **Date Solved:** March 4, 2024 12:47 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/spiral-matrix/description/
- **Algorithm:** brute force
- **Type:** matrix, scenario

## Problem Statement

Given an m x n matrix, return all elements of the matrix in spiral order.

## Constraints


- m == matrix.length
- n == matrix[i].length
- 1 <= m, n <= 10
- -100 <= matrix[i][j] <= 100

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I tried way too hard to solve this by traversal and playing tricks with matrix indices. All along I should have thought of this similar to a graph problem. Marking positions visited and iterating through the well defined set of directions to make a spiral (left, down, right, up…DOH!!!!).

Im quite frustrated and disappointed I was not able to think more along these lines without having to look at answers/hints. This one hurts…


```go
package main

var ldru = [][2]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

const Visited = -101

func spiralOrder(matrix [][]int) []int {
	N, M := len(matrix), len(matrix[0])
	ans := []int{matrix[0][0]}

	var pos, move, nextPos [2]int
	var currentDir int

	matrix[0][0] = Visited
	pos = [2]int{0, 0}
	for len(ans) < N*M {
		move = ldru[currentDir%4]
		nextPos = [2]int{pos[0] + move[0], pos[1] + move[1]}
		if nextValid(N, M, nextPos) && matrix[nextPos[0]][nextPos[1]] != Visited {
			pos = nextPos
			ans = append(ans, matrix[pos[0]][pos[1]])
			matrix[pos[0]][pos[1]] = Visited
		} else {
			currentDir++
		}
	}
	return ans
}

func nextValid(N, M int, pos [2]int) bool {
	return pos[0] >= 0 && pos[0] < N && pos[1] >= 0 && pos[1] < M
}
```
