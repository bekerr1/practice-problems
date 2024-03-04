# Find the Largest Area of Square Inside Two Rectangles

## Metadata

- **Date Solved:** March 3, 2024 7:06 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-largest-area-of-square-inside-two-rectangles/description/
- **Algorithm:** brute force
- **Type:** math, scenario

## Problem Statement

There exist n rectangles in a 2D plane. You are given two 0-indexed 2D integer arrays bottomLeft and topRight, both of size n x 2, where bottomLeft[i] and topRight[i]represent the bottom-left and top-right coordinates of the ith rectangle respectively.

You can select a region formed from the intersection of two of the given rectangles. You need to find the largest area of a square that can fit inside this region if you select the region optimally.

Return the largest possible area of a square, or 0 if there do not exist any intersecting regions between the rectangles.

## Constraints


- n == bottomLeft.length == topRight.length
- 2 <= n <= 103
- bottomLeft[i].length == topRight[i].length == 2
- 1 <= bottomLeft[i][0], bottomLeft[i][1] <= 107
- 1 <= topRight[i][0], topRight[i][1] <= 107
- bottomLeft[i][0] < topRight[i][0]
- bottomLeft[i][1] < topRight[i][1]

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I ended up getting most of the intuition when calculating the overlapping square. I tried to come up with an optimized solution by sorting the points and iterating through them in an attempt to only consider the necessary points (that would likely overlap). This was a mistake as the logic to move to the next point missed the solution and I was not able to come up with an algorithm to account for this.

The final solution was to iterate through all points and come up with the max square by trying everything.


```go
package main

import "math"

func largestSquareArea(bottomLeft [][]int, topRight [][]int) int64 {
	N := len(bottomLeft)
	maxSquare := 0
	for i := 0; i < N; i++ {
		for j := i + 1; j < N; j++ {
			lowerX := max(bottomLeft[i][0], bottomLeft[j][0])
			lowerY := max(bottomLeft[i][1], bottomLeft[j][1])
			upperX := min(topRight[i][0], topRight[j][0])
			upperY := min(topRight[i][1], topRight[j][1])

			if lowerX >= upperX || lowerY >= upperY {
				continue
			}

			x := upperX - lowerX
			y := upperY - lowerY

			maxSquare = max(maxSquare, min(x, y))
		}
	}
	if maxSquare == math.MaxInt {
		return 0
	}
	return int64(maxSquare * maxSquare)
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
