# Maximize the Profit as the Salesman

## Metadata

- **Date Solved:** August 24, 2023 5:50 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximize-the-profit-as-the-salesman/description/
- **Algorithm:** binary search, dynamic programming, graph
- **Type:** array, scenario

## Problem Statement

You are given an integer n representing the number of houses on a number line, numbered from 0 to n - 1.

Additionally, you are given a 2D integer array offers where offers[i] = [starti, endi, goldi], indicating that ith buyer wants to buy all the houses from starti to endi for goldi amount of gold.

As a salesman, your goal is to maximize your earnings by strategically selecting and selling houses to buyers.

Return the maximum amount of gold you can earn.

Note that different buyers can't buy the same house, and some houses may remain unsold.

## Constraints

- 1 <= n <= 105
- 1 <= offers.length <= 105
- offers[i].length == 3
- 0 <= starti <= endi <= n - 1
- 1 <= goldi <= 103

## Solution

- **Status:** Looked At Discuss

### Solution Notes

My iterative approach was accurate but off. I was recursing the wrong way and adding up values incorrectly. I was also caching the wrong value, though it would have still worked. Additionally, it is hitting TLE on case 525.

The adjacency list approach is an easy next step from my iterative approach.

Binary search would be hard for me to come up with naturally without many hints. I would not expect to come up with that if this was given in an interview.


### Iterative DP (TLE)

```go
package main

import "sort"

func maximizeTheProfit(n int, offers [][]int) int {
	memo = make(map[int]int)
	sort.Slice(offers, func(i, j int) bool {
		if offers[i][0] == offers[j][0] {
			return offers[i][1] < offers[j][1]
		}
		return offers[i][0] < offers[j][0]
	})
	return maximizeTheProfitUtil(offers, 0, offers[0])
}

var memo map[int]int

func maximizeTheProfitUtil(offers [][]int, start int, curOffer []int) int {
	if start == len(offers)-1 {
		return curOffer[2]
	}

	if v, exists := memo[start]; exists {
		return v
	}

	take := curOffer[2]
	notTake := 0
	m := 0
	for i := start + 1; i < len(offers); i++ {
		if offers[i][0] > curOffer[1] {
			take = curOffer[2] + maximizeTheProfitUtil(offers, i, offers[i])
		} else {
			notTake = max(maximizeTheProfitUtil(offers, i, offers[i]), notTake)
		}
		m = max(m, max(take, notTake))
	}
	memo[start] = m
	return memo[start]
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Iterative DP with Adjacency List

Really cool way to solve this problem!!

```go
package main

func maximizeTheProfit(n int, offers [][]int) int {
	memo = make(map[int]int, n)
	adjList := make([][][2]int, n)
	for _, offer := range offers {
		adjList[offer[0]] = append(adjList[offer[0]], [2]int{offer[1], offer[2]})
	}
	return maximizeTheProfitUtil(n, 0, adjList)
}

var memo map[int]int

func maximizeTheProfitUtil(n, idx int, adj [][][2]int) int {
	if idx >= n {
		return 0
	}
	if v, exists := memo[idx]; exists {
		return v
	}

	notTake := maximizeTheProfitUtil(n, idx+1, adj)
	take := 0
	if len(adj[idx]) > 0 {
		for _, a := range adj[idx] {
			end := a[0]
			offer := a[1]
			take = max(take, offer+maximizeTheProfitUtil(n, end+1, adj))
		}
	}
	memo[idx] = max(take, notTake)
	return memo[idx]
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Binary Search DP
