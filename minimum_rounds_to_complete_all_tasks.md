# Minimum Rounds to Complete All Tasks

## Metadata

- **Date Solved:** September 20, 2022 9:10 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-rounds-to-complete-all-tasks/
- **Algorithm:** greedy, hashmap
- **Type:** array

## Problem Statement

You are given a 0-indexed integer array tasks, where tasks[i] represents the difficulty level of a task. In each round, you can complete either 2 or 3 tasks of the same difficulty level.
Return the minimum rounds required to complete all the tasks, or -1 if it is not possible to complete all the tasks.

## Constraints

- 1 <= tasks.length <= 105
- 1 <= tasks[i] <= 109

## Solution

- **Status:** Solved On Own


```go
package main

func minimumRounds(tasks []int) int {

	countMap := make(map[int]int)
	for i := range tasks {
		countMap[tasks[i]]++
	}

	rounds := 0
	for _, count := range countMap {
		if count == 1 {
			return -1
		}
		for count%3 != 0 {
			count -= 2
			rounds++
		}
		rounds += count / 3
	}
	return rounds
}
```
