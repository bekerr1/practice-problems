# Possible Bipartition

## Metadata

- **Date Solved:** December 27, 2022 10:03 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/possible-bipartition/description/
- **Algorithm:** bfs, dfs, union find
- **Type:** graph

## Problem Statement

We want to split a group of n people (labeled from 1 to n) into two groups of any size. Each person may dislike some other people, and they should not go into the same group.
Given the integer n and the array dislikes where dislikes[i] = [ai, bi] indicates that the person labeled ai does not like the person labeled bi, return true if it is possible to split everyone into two groups in this way.

## Constraints

- 1 <= n <= 2000
- 0 <= dislikes.length <= 104
- dislikes[i].length == 2
- 1 <= dislikes[i][j] <= n
- ai < bi
- All the pairs of dislikes are unique.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I thought this was a directed graph problem but it wasnt. If a dislikes b, b cannot un-dislike itself in terms of a, so there is not a directed path from a to b, its undirected and explicit. 
This graph problem turned into a bipartite one, which i did not know of.


### DFS

```go
package main

var colors []int

func possibleBipartition(n int, dislikes [][]int) bool {
	colors = make([]int, n+1)
	adjList := make([][]int, n+1)

	for _, dislike := range dislikes {
		adjList[dislike[0]] = append(adjList[dislike[0]], dislike[1])
		adjList[dislike[1]] = append(adjList[dislike[1]], dislike[0])
	}

	for i := 1; i <= n; i++ {
		if !dfs(i, adjList) {
			return false
		}
	}
	return true
}

func dfs(cur int, adjList [][]int) bool {
	if colors[cur] == 0 {
		colors[cur] = 1
	}
	for _, n := range adjList[cur] {
		if colors[n] == 0 {
			colors[n] = -colors[cur]
			dfs(n, adjList)
		} else if colors[cur] == colors[n] {
			return false
		}
	}
	return true
}
```
