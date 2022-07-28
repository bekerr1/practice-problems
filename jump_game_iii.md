# Jump Game III

## Metadata

- **Date Solved:** July 27, 2022 10:58 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/jump-game-iii/
- **Algorithm:** bfs
- **Type:** array

## Problem Statement

Given an array of non-negative integers arr, you are initially positioned at start index of the array. When you are at index i, you can jump to i + arr[i] or i - arr[i], check if you can reach to any index with value 0.
Notice that you can not jump outside of the array at any time.

## Problem


```go
package main

func canReach(arr []int, start int) bool {
	visited := make(map[int]struct{})

	queue := make([]int, 0, len(arr))
	queue = append(queue, start)

	for len(queue) > 0 {
		curIdx := queue[0]
		queue = queue[1:]

		visited[curIdx] = struct{}{}

		if arr[curIdx] == 0 {
			return true
		}

		right := curIdx + arr[curIdx]
		_, visitedRight := visited[right]
		if right < len(arr) && !visitedRight {
			queue = append(queue, right)
		}

		left := curIdx - arr[curIdx]
		_, visitedLeft := visited[left]
		if left >= 0 && !visitedLeft {
			queue = append(queue, left)
		}
	}
	return false
}
```
