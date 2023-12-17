# Maximum Path Quality of a Graph

## Metadata

- **Date Solved:** December 17, 2023 11:16 AM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/maximum-path-quality-of-a-graph/description/
- **Algorithm:** dfs
- **Type:** graph

## Problem Statement

There is an undirected graph with n nodes numbered from 0 to n - 1 (inclusive). You are given a 0-indexed integer array values where values[i] is the value of the ithnode. You are also given a 0-indexed 2D integer array edges, where each edges[j] = [uj, vj, timej] indicates that there is an undirected edge between the nodes uj and vj, and it takes timej seconds to travel between the two nodes. Finally, you are given an integer maxTime.

A valid path in the graph is any path that starts at node 0, ends at node 0, and takes at most maxTime seconds to complete. You may visit the same node multiple times. The quality of a valid path is the sum of the values of the unique nodes visited in the path (each node's value is added at most once to the sum).

Return the maximum quality of a valid path.

Note: There are at most four edges connected to each node.

## Constraints

- n == values.length
- 1 <= n <= 1000
- 0 <= values[i] <= 108
- 0 <= edges.length <= 2000
- edges[j].length == 3
- 0 <= uj < vj <= n - 1
- 10 <= timej, maxTime <= 100
- All the pairs [uj, vj] are unique.
- There are at most four edges connected to each node.
- The graph may not be connected.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

The constraints here are what allowed for this problem to be solved in this way. Because time ≥ 10 and maxTime ≤ 100, you would only ever get at most 100/10 recursive calls per edge. Additionally, there are at most four edges connected to each node.

For me I had trouble thinking of a solution that would traverse all paths. Actually, normal DFS without qualifying an edge as visited before traversing it does just this. No longer does it matter if an edge is visited but if we are in the allotted time. In this way, we traverse all iterations of paths.


```go
package main

func maximalPathQuality(values []int, edges [][]int, maxTime int) int {
	ans := 0
	visited := make(map[int]int)
	adjList := make([][][2]int, len(values))
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], [2]int{e[1], e[2]})
		adjList[e[1]] = append(adjList[e[1]], [2]int{e[0], e[2]})
	}
	var dfs func(int, int, int)
	dfs = func(quality, curTime, node int) {
		if visited[node] == 0 {
			quality += values[node]
		}
		visited[node]++
		if node == 0 {
			ans = max(ans, quality)
		}
		for _, e := range adjList[node] {
			if e[1]+curTime <= maxTime {
				dfs(quality, e[1]+curTime, e[0])
			}
		}
		visited[node]--
	}
	dfs(0, 0, 0)
	return ans
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
