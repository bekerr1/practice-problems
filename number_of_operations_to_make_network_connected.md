# Number of Operations to Make Network Connected

## Metadata

- **Date Solved:** December 15, 2023 9:12 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/number-of-operations-to-make-network-connected/description/
- **Algorithm:** union find
- **Type:** graph

## Problem Statement

There are n computers numbered from 0 to n - 1 connected by ethernet cables connections forming a network where connections[i] = [ai, bi]represents a connection between computers ai and bi. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network connections. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return the minimum number of times you need to do this in order to make all the computers connected. If it is not possible, return -1.

## Constraints

- 1 <= n <= 105
- 1 <= connections.length <= min(n * (n - 1) / 2, 105)
- connections[i].length == 2
- 0 <= ai, bi < n
- ai != bi
- There are no repeated connections.
- No two computers are connected by more than one cable.

## Solution

- **Status:** Solved On Own

### Solution Notes

I solved this on my own with some rough edges. One aspect I wished I got better was how to deal with already used edges. In my initial implementation I counted extra edges and removed an edge after using it. A better solution was to start with the number of connections and as I created unions when I encounter two disjoint nodes who were neighbors that would become connected I would decrement the number of connections. In this way, when encountering an already used edge, we would indicate a union has already been made and not decrement the connection as it was already done when we first made the union.

With this approach I also would not have to account for groups as the number of extra connections would already be known at the end. Additionally, checking the number of connections and ensuring the amount is greater than n-1 could have accounted for the -1 case.


```go
package main

func makeConnected(n int, connections [][]int) int {
	if len(connections) < n-1 {
		return -1
	}

	adjList := make([][]int, n)
	for _, c := range connections {
		adjList[c[0]] = append(adjList[c[0]], c[1])
		adjList[c[1]] = append(adjList[c[1]], c[0])
	}

	parent = make(map[int]int)
	rank = make([]int, n)
	conns := n - 1
	for i := 0; i < n; i++ {
		for _, e := range adjList[i] {
			pu := find(i)
			pe := find(e)
			if pu != pe {
				conns--
			}
			union(e, i)
		}
	}
	return conns
}

var parent map[int]int
var rank []int

func union(n1, n2 int) {
	n1p, n2p := find(n1), find(n2)
	if n1p == n2p {
		return
	}
	switch {
	case rank[n1p] < rank[n2p]:
		parent[n1p] = n2p
	case rank[n1p] > rank[n2p]:
		parent[n2p] = n1p
	default:
		parent[n1p] = n2p
		rank[n2p]++
	}
}

func find(n int) int {
	if p, ex := parent[n]; !ex || n == p {
		return n
	}
	parent[n] = find(parent[n])
	return parent[n]
}
```
