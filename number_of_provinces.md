# Number of Provinces

## Metadata

- **Date Solved:** March 22, 2025 10:30 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/number-of-provinces/description/
- **Algorithm:** union find
- **Type:** graph

## Problem Statement

There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

## Constraints


- 1 <= n <= 200
- n == isConnected.length
- n == isConnected[i].length
- isConnected[i][j] is 1 or 0.
- isConnected[i][i] == 1
- isConnected[i][j] == isConnected[j][i]

## Solution

- **Status:** Solved On Own


```go
package main

func findCircleNum(isConnected [][]int) int {
	components := len(isConnected)
	initDS(components)
	for i, c := range isConnected {
		for j, conn := range c {
			if conn == 1 && find(i) != find(j) {
				components--
				union(i, j)
			}
		}
	}
	return components
}

var parents []int // to track connected components
var rank []int    // for rank heuristics
func initDS(size int) {
	parents, rank = make([]int, size), make([]int, size)
	// Every node starts out as its own parent
	for i := range parents {
		parents[i] = i
	}
}

func union(n1, n2 int) {
	p1, p2 := find(n1), find(n2)
	// These two nodes are already unioned together
	if p1 == p2 {
		return
	}
	// Add the smaller ranked component to the larger one.
	// Situate the vars such that p1 is higher ranked.
	if rank[p1] < rank[p2] {
		p1, p2 = p2, p1
	}
	parents[p2] = p1
	// If ranks are equal, increase p1's rank due to
	// it being designated the parent of p2.
	if rank[p1] == rank[p2] {
		rank[p1]++
	}
}

// Path compression version of find
func find(n int) int {
	p := parents[n]
	if p == n {
		return n
	}
	parents[n] = find(p)
	return parents[n]
}
```
