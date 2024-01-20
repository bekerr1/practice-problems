# Critical Connections in a Network

## Metadata

- **Date Solved:** January 20, 2024 11:31 AM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/critical-connections-in-a-network/
- **Algorithm:** dfs
- **Type:** graph

## Problem Statement

There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network.

A critical connection is a connection that, if removed, will make some servers unable to reach some other server.
Return all critical connections in the network in any order.

## Constraints

- 2 <= n <= 105
- n - 1 <= connections.length <= 105
- 0 <= ai, bi <= n - 1
- ai != bi
- There are no repeated connections.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I was able to get the base intuition in that detecting a cycle was the goal here. Additionally, I had a skeleton of the correct algorithm in that you would return some value from the DFS and use it to remove connections that were within the cycle. My attempt though was incorrect. I tried to use the parent node which did not work for all cycle connections.

The correct approach is to use a rank. The rank map would serve as a 'visited' set along with 'when' the node was visited. If the rank returned was ever ≤ to the rank local to the stack frame, this means the node returning that rank had been seen. We would then recurse until we return a rank > stack local one, as this would mean we are returning the child's rank, as the rank is incremented on all DFS calls.

The minRank value is also a subtlety. It is required for when we encounter a cycle and then possibly another cycle with a smaller rank than the first. In this case, we would need to recurse farther back than we would have for the first cycle rank encountered.


```go
package main

func criticalConnections(n int, connections [][]int) [][]int {

	adjList := make([][]int, n)
	connectionMap := make(map[[2]int]bool)
	for _, c := range connections {
		adjList[c[0]] = append(adjList[c[0]], c[1])
		adjList[c[1]] = append(adjList[c[1]], c[0])
		connectionMap[[2]int{c[0], c[1]}] = true
	}

	rank := make(map[int]int)
	var dfs func(int, int) int
	dfs = func(u, r int) int {
		if _, ok := rank[u]; ok {
			return rank[u]
		}
		rank[u] = r
		minRank := r + 1
		for _, v := range adjList[u] {
			// Skip Parent
			if vr, ok := rank[v]; ok && vr == r-1 {
				continue
			}
			rr := dfs(v, r+1)
			if rr <= r {
				delete(connectionMap, [2]int{u, v})
				delete(connectionMap, [2]int{v, u})
			}
			// Reassign minRank in-case we get a recursiveRank
			// less than the current. This can happen multiple
			// times in the case we encounter multiple cycles
			minRank = min(minRank, rr)
		}
		return minRank
	}

	dfs(0, 0)

	ans := [][]int{}
	for c := range connectionMap {
		ans = append(ans, []int{c[0], c[1]})
	}
	return ans
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
