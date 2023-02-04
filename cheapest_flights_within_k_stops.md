# Cheapest Flights Within K Stops

## Metadata

- **Date Solved:** February 4, 2023 11:36 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/cheapest-flights-within-k-stops/description/
- **Algorithm:** dijkstra
- **Type:** graph

## Problem Statement

There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.
You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

## Constraints

- 1 <= n <= 100
- 0 <= flights.length <= (n * (n - 1) / 2)
- flights[i].length == 3
- 0 <= fromi, toi < n
- fromi != toi
- 1 <= pricei <= 104
- There will not be any multiple flights between two cities.
- 0 <= src, dst, k < n
- src != dst

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Keeping track of the number of steps in order to avoid going down a path that already has less steps was key.
Additionally, keeping the ‘K’ value for each node was required to ensure the step count was accounted for.


```go
package main

import "container/heap"

// dest, price, k is the triplet
type TripHeap [][3]int

func (h TripHeap) Len() int           { return len(h) }
func (h TripHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h TripHeap) Less(i, j int) bool { return h[i][1] < h[j][1] }

func (h *TripHeap) Push(x interface{}) {
	*h = append(*h, x.([3]int))
}

func (h *TripHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

const (
	FromIdx  = 0
	ToIdx    = 1
	PriceIdx = 2
)

func findCheapestPrice(n int, flights [][]int, src int, dst int, k int) int {
	steps := [101]int{}
	adjList := make([][][2]int, len(flights)+1)
	for _, flight := range flights {
		adjList[flight[FromIdx]] = append(adjList[flight[FromIdx]], [2]int{
			flight[ToIdx],
			flight[PriceIdx],
		})
	}
	pq := &TripHeap{[3]int{src, 0, k + 1}}
	heap.Init(pq)

	for pq.Len() > 0 {
		cur := heap.Pop(pq).([3]int)
		u := cur[0]
		w0 := cur[1]
		K := cur[2]
		if steps[u] > K {
			continue
		}
		steps[u] = K
		if u == dst {
			return w0
		}
		if K > 0 {
			for _, dest := range adjList[u] {
				v := dest[0]
				w := dest[1]
				heap.Push(pq, [3]int{v, w0 + w, K - 1})
			}
		}
	}
	return -1
}
```
