# Insert Interval

## Metadata

- **Date Solved:** August 29, 2022 10:33 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/insert-interval/
- **Algorithm:** brute force
- **Type:** intervals

## Problem Statement

You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.
Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).
Return intervals after the insertion.

## Constraints

- 0 <= intervals.length <= 104
- intervals[i].length == 2
- 0 <= starti <= endi <= 105
- intervals is sorted by starti in ascending order.
- newInterval.length == 2
- 0 <= start <= end <= 105

## Solution


```go
package main

func insert(intervals [][]int, newInterval []int) [][]int {
	newIntervals := make([][]int, 0, len(intervals)+1)
	for i := 0; i < len(intervals); i++ {
		if intervals[i][1] >= newInterval[0] && intervals[i][0] <= newInterval[1] {
			newInterval[0] = min(intervals[i][0], newInterval[0])
			newInterval[1] = max(intervals[i][1], newInterval[1])
		} else {
			if intervals[i][0] > newInterval[0] {
				swap(intervals[i], newInterval)
			}
			newIntervals = append(newIntervals, intervals[i])
		}
	}
	newIntervals = append(newIntervals, newInterval)
	return newIntervals
}

func swap(inter1, inter2 []int) {
	inter1[0], inter2[0] = inter2[0], inter1[0]
	inter1[1], inter2[1] = inter2[1], inter1[1]
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
