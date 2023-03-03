# Minimum Fuel Cost to Report to the Capital

## Metadata

- **Date Solved:** March 3, 2023 8:48 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-fuel-cost-to-report-to-the-capital/description/
- **Algorithm:** dfs
- **Type:** graph, tree

## Problem Statement

There is a tree (i.e., a connected, undirected graph with no cycles) structure country network consisting of n cities numbered from 0 to n - 1 and exactly n - 1 roads. The capital city is city 0. You are given a 2D integer array roads where roads[i] = [ai, bi] denotes that there exists a bidirectional road connecting cities ai and bi.
There is a meeting for the representatives of each city. The meeting is in the capital city.
There is a car in each city. You are given an integer seats that indicates the number of seats in each car.
A representative can use the car in their city to travel or change the car and ride with another representative. The cost of traveling between two cities is one liter of fuel.
Return the minimum number of liters of fuel to reach the capital city.

## Constraints

- 1 <= n <= 105
- roads.length == n - 1
- roads[i].length == 2
- 0 <= ai, bi < n
- ai != bi
- roads represents a valid tree.
- 1 <= seats <= 105

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I tried to solve this problem too literal yet again. Trying to juggle which nodes are filled exactly and having to mange child nodes to pass to grandparent nodes when parent nodes are full was way too much.
Just considering the statement, “How many cars are needed when we have r representatives that need to go from node N to parent node P”. The answer to this would be, ceil(r/seats), because we can fit r representatives in ceil(r/seats) cars. Using this more general statement, its much easier to calculate the liters driven, as each car would drive 1 liter.


```go
package main

import "math"

var litersUsed int64

func minimumFuelCost(roads [][]int, seats int) int64 {
	adjList := make([][]int, len(roads)+1)
	litersUsed = 0
	for _, road := range roads {
		adjList[road[0]] = append(adjList[road[0]], road[1])
		adjList[road[1]] = append(adjList[road[1]], road[0])
	}
	for _, r := range adjList[0] {
		dfs(0, r, seats, adjList)
	}
	return litersUsed
}

func dfs(parent, road, seats int, adjList [][]int) int {
	representatives := 1
	for _, r := range adjList[road] {
		if r != parent {
			representatives += dfs(road, r, seats, adjList)
		}
	}
	litersUsed += int64(math.Ceil(float64(representatives) / float64(seats)))
	return representatives
}
```
