# Container With Most Water

## Metadata

- **Date Solved:** April 9, 2022 10:02 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/container-with-most-water/
- **Algorithm:** greedy
- **Type:** array

## Problem Statement

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).
Find two lines that together with the x-axis form a container, such that the container contains the most water.
Return the maximum amount of water a container can store.
Notice that you may not slant the container.

## Problem


```go
package main

func maxArea(height []int) int {
	var (
		left  int
		right int = len(height) - 1
		area      = min(height[left], height[right]) * (right - left)
	)

	for left < right {
		numLeft := height[left]
		numRight := height[right]
		if numLeft < numRight {
			left++
		} else {
			right--
		}

		localArea := min(height[left], height[right]) * (right - left)
		if localArea > area {
			area = localArea
		}
	}
	return area
}

func min(x, y int) int {
	if x < y {
		return x
	} else {
		return y
	}
}
```
