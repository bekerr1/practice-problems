# K Closest Points to Origin

## Metadata

- **Date Solved:** February 13, 2022 10:58 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/k-closest-points-to-origin/
- **Algorithm:** heap, sort
- **Type:** array

## Problem Statement

Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane and an integer k, return the k closest points to the origin (0, 0).
The distance between two points on the X-Y plane is the Euclidean distance (i.e., √(x1 - x2)2 + (y1 - y2)2).
You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

```go
package main

import (
	"container/heap"
	"math"
)

func kClosest(points [][]int, k int) [][]int {
	pointHeap := &PointHeap{}
	heap.Init(pointHeap)
	for _, point := range points {
		heapPoint := Point{
			point:            point,
			distanceFromZero: euclideanDistanceFromZero(point),
		}
		heap.Push(pointHeap, heapPoint)
		if pointHeap.Len() > k {
			heap.Pop(pointHeap)
		}

	}
	return pointHeap.Points()
}

func euclideanDistanceFromZero(point []int) float64 {
	return math.Sqrt(math.Pow(float64(point[0]-0), 2.0) + math.Pow(float64(point[1]-0), 2.0))
}

type Point struct {
	point            []int
	distanceFromZero float64
}

// An PointHeap is a min-heap of Points.
type PointHeap []Point

func (h PointHeap) Points() [][]int {
	ret := [][]int{}
	for _, pt := range h {
		ret = append(ret, pt.point)
	}
	return ret
}
func (h PointHeap) Len() int { return len(h) }
func (h PointHeap) Less(i, j int) bool {
	if h[i].distanceFromZero > h[j].distanceFromZero {
		return true
	}
	return false
}
func (h PointHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }

func (h *PointHeap) Push(x interface{}) {
	*h = append(*h, x.(Point))
}

func (h *PointHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}
```

### Solution Notes
