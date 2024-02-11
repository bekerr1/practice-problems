# Robot Room Cleaner

## Metadata

- **Date Solved:** February 11, 2024 11:44 AM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/robot-room-cleaner/description/
- **Algorithm:** backtracking, dfs
- **Type:** scenario

## Problem Statement

You are controlling a robot that is located somewhere in a room. The room is modeled as an m x n binary grid where 0 represents a wall and 1 represents an empty slot.

The robot starts at an unknown location in the room that is guaranteed to be empty, and you do not have access to the grid, but you can move the robot using the given API Robot.

You are tasked to use the robot to clean the entire room (i.e., clean every empty cell in the room). The robot with the four given APIs can move forward, turn left, or turn right. Each turn is 90 degrees.

When the robot tries to move into a wall cell, its bumper sensor detects the obstacle, and it stays on the current cell.

## Constraints

- m == room.length
- n == room[i].length
- 1 <= m <= 100
- 1 <= n <= 200
- room[i][j] is either 0 or 1.
- 0 <= row < m
- 0 <= col < n
- room[row][col] == 1
- All the empty cells can be visited from the starting position.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

The two key intuitions here were to use constrained programming to limit the possible combinations of moves to perform and backtracking to handle when you reach a dead-end. 

Constrained programming is implemented by only turning right and having the movements follow. This creates a sort of spiral graph traversal.

Backtracking is implemented by doing a recursion and when a dead end is reached, returning from the recursion and going back a cell. To go back, we turn right twice, move, then turn right twice again to situate to the position we were in, then continue.


```go
package main

var movements = [][2]int{{-1, 0}, {0, 1}, {1, 0}, {0, -1}}
var visited map[[2]int]bool

func cleanRoom(robot *Robot) {
	visited = make(map[[2]int]bool)
	backtrack([2]int{0, 0}, 0, robot)
}

func backtrack(loc [2]int, dir int, robot *Robot) {
	visited[loc] = true
	robot.Clean()

	for i := 0; i < len(movements); i++ {
		newDir := (dir + i) % len(movements)
		move := movements[newDir]
		newLoc := [2]int{loc[0] + move[0], loc[1] + move[1]}
		if !visited[newLoc] && robot.Move() {
			backtrack(newLoc, newDir, robot)
			goBack(robot)
		}
		robot.TurnRight()
	}
}

func goBack(robot *Robot) {
	robot.TurnRight()
	robot.TurnRight()
	robot.Move()
	robot.TurnRight()
	robot.TurnRight()
}
```
