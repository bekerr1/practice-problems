# Jump Game IV

## Metadata

- **Date Solved:** July 30, 2022 11:28 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/jump-game-iv/
- **Algorithm:** bfs
- **Type:** array

## Problem Statement

Given an array of integers arr, you are initially positioned at the first index of the array.
In one step you can jump from index i to index:
- i + 1 where: i + 1 < arr.length.
- i - 1 where: i - 1 >= 0.
- j where: arr[i] == arr[j] and i != j.
Return the minimum number of steps to reach the last index of the array.
Notice that you can not jump outside of the array at any time.

## Problem


```go
package main

func minJumps(arr []int) int {
	var (
		visited   = make(map[int]struct{})
		valueSeen = make(map[int]struct{})
		queue     = make([]int, 0)
		valueMap  = make(map[int][]int)
	)

	for i := len(arr) - 1; i >= 0; i-- {
		valueMap[arr[i]] = append(valueMap[arr[i]], i)
	}

	queue = append(queue, 0)
	visited[0] = struct{}{}
	steps := 0

	for len(queue) > 0 {
		stepLen := len(queue)
		for stepLen > 0 {
			stepLen--
			cur := queue[0]
			queue = queue[1:]
			if cur == len(arr)-1 {
				return steps
			}
			if _, seen := valueSeen[arr[cur]]; !seen {
				valueSeen[arr[cur]] = struct{}{}
				for _, edge := range valueMap[arr[cur]] {
					if _, visit := visited[edge]; !visit {
						visited[edge] = struct{}{}
						queue = append(queue, edge)
					}
				}
			}
			if cur > 0 {
				if _, visit := visited[cur-1]; !visit {
					visited[cur-1] = struct{}{}
					queue = append(queue, cur-1)
				}
			}
			if cur < len(arr)-1 {
				if _, visit := visited[cur+1]; !visit {
					visited[cur+1] = struct{}{}
					queue = append(queue, cur+1)
				}
			}
		}
		steps++
	}
	return steps
}
```

## Solution Statement


The catch here was to both strictly manage the queue for already visited entries and maintain a set of already seen arr values so as to not have to run through the valueMap multiple times. Additionally, a key way to keep BFS distance given the distance is simply ‘steps’ is to iterate through the full ‘length’ of the queue at the beginning of every queue entry. This way you can know every cur entry is the next full iteration of entries.
