# Number of Good Paths

## Metadata

- **Date Solved:** December 14, 2023 11:01 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/number-of-good-paths/description/
- **Algorithm:** hashmap, sort, union find
- **Type:** graph, math

## Problem Statement

There is a tree (i.e. a connected, undirected graph with no cycles) consisting of n nodes numbered from 0 to n - 1and exactly n - 1 edges.

You are given a 0-indexed integer array vals of length nwhere vals[i] denotes the value of the ith node. You are also given a 2D integer array edges where edges[i] = [ai, bi] denotes that there exists an undirected edge connecting nodes ai and bi.

A good path is a simple path that satisfies the following conditions:
1. The starting node and the ending node have the samevalue.
2. All nodes between the starting node and the ending node have values 
less than or equal to the starting node (i.e. the starting node's value should be the maximum value along the path).

Return the number of distinct good paths.

Note that a path and its reverse are counted as the samepath. For example, 0 -> 1 is considered to be the same as 1 -> 0. A single node is also considered as a valid path.

## Constraints

- n == vals.length
- 1 <= n <= 3 * 104
- 0 <= vals[i] <= 105
- edges.length == n - 1
- edges[i].length == 2
- 0 <= ai, bi < n
- ai != bi
- edges represents a valid tree.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

This problem was quite tricky. The intuition here that could have served me good was to think about building, from bottom-up, disjoint sets of graphs that met the constraints described.

My first thought was to sort the the nodes by value and traverse from lower nodes to try to 'manually' discover the paths. This would take complex conditional logic and probably would not have passed in the end.

Using union-find here to make low nodes as representatives and count the groups was key. I was also on-track with the math and how to come up with the answer. One quote from the solutions I liked…

”If you see something with tree and path/subtree and something like value less than some edge or nodes, then it is possible to think about a solution related to union find by adding edges in some order.” 


```go
package main

import "sort"

func numberOfGoodPaths(vals []int, edges [][]int) int {
	parent, rank = make(map[int]int, len(vals)), make([]int, len(vals))
	idxForVals := make(map[int][]int, len(vals))
	vss := []int{}
	for i, v := range vals {
		if _, exist := idxForVals[v]; !exist {
			idxForVals[v] = make([]int, 0)
			vss = append(vss, v)
		}
		idxForVals[v] = append(idxForVals[v], i)
	}
	sort.Ints(vss)

	adjList := make([][]int, len(vals))
	for _, e := range edges {
		adjList[e[0]] = append(adjList[e[0]], e[1])
		adjList[e[1]] = append(adjList[e[1]], e[0])
	}

	ans := 0
	for _, v := range vss {
		for _, node := range idxForVals[v] {
			for _, e := range adjList[node] {
				if vals[node] >= vals[e] {
					union(e, node)
				}
			}
		}

		groupByCount := make(map[int]int)
		for _, node := range idxForVals[v] {
			groupByCount[find(node)]++
		}
		for _, v := range groupByCount {
			ans += (v * (v + 1)) / 2
		}
	}
	return ans
}

var parent map[int]int
var rank []int

func union(n1, n2 int) {
	n1P := find(n1)
	n2P := find(n2)

	if n1P == n2P {
		return
	}
	switch {
	case rank[n1P] > rank[n2P]:
		parent[n2P] = n1P
	case rank[n1P] < rank[n2P]:
		parent[n1P] = n2P
	default:
		parent[n1P] = n2P
		rank[n2P]++
	}
}

func find(n int) int {
	if v, exists := parent[n]; !exists || v == n {
		return n
	}
	parent[n] = find(parent[n])
	return parent[n]
}
```
