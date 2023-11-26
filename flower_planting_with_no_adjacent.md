# Flower Planting With No Adjacent

## Metadata

- **Date Solved:** November 26, 2023 12:43 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/flower-planting-with-no-adjacent/
- **Algorithm:** bfs
- **Type:** graph

## Problem Statement

You have n gardens, labeled from 1 to n, and an array paths where paths[i] = [xi, yi] describes a bidirectional path between garden xi to garden yi. In each garden, you want to plant one of 4 types of flowers.
All gardens have at most 3 paths coming into or leaving it.
Your task is to choose a flower type for each garden such that, for any two gardens connected by a path, they have different types of flowers.

Return any such a choice as an array answer, where answer[i] is the type of flower planted in the (i+1)th garden. The flower types are denoted 1, 2, 3, or 4. It is guaranteed an answer exists.

## Constraints

- 1 <= n <= 104
- 0 <= paths.length <= 2 * 104
- paths[i].length == 2
- 1 <= xi, yi <= n
- xi != yi
- Every garden has at most 3 paths coming into or leaving it.

## Solution

- **Status:** Needed Hints

### Solution Notes

Realizing the constraint of 'at most 3 paths coming into or leaving a node' allowed for always having an option to have an adj node with a different type was the key. 

After this, coming up with a way to track and get an available type lead to the solution.


```go
package main

func gardenNoAdj(n int, paths [][]int) []int {
	adjList := make([][]int, n+1)
	for _, p := range paths {
		adjList[p[0]] = append(adjList[p[0]], p[1])
		adjList[p[1]] = append(adjList[p[1]], p[0])
	}

	answer := make([]int, n+1)
	for i := 1; i <= n; i++ {
		if answer[i] > 0 {
			continue
		}
		queue := []int{i}
		for len(queue) > 0 {
			curTypeTaken := [4]bool{false, false, false, false}
			cur := queue[0]
			queue = queue[1:]
			if answer[cur] > 0 {
				continue
			}
			for _, n := range adjList[cur] {
				if answer[n] == 0 {
					queue = append(queue, n)
				} else {
					curTypeTaken[answer[n]-1] = true
				}
			}
			for i, taken := range curTypeTaken {
				if !taken {
					answer[cur] = i + 1
				}
			}
		}
	}
	return answer[1:]
}
```
