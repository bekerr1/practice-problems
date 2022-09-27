# H-Index II

## Metadata

- **Date Solved:** September 27, 2022 9:17 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/h-index-ii/
- **Algorithm:** binary search
- **Type:** array

## Problem Statement

Given an array of integers citations where citations[i] is the number of citations a researcher received for their ith paper and citations is sorted in an ascending order, return compute the researcher's h-index.

## Constraints

- n == citations.length
- 1 <= n <= 105
- 0 <= citations[i] <= 1000
- citations is sorted in ascending order

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Clarifying questions matter. Because of the clarifying question, best approach compares the citations[mid] to the current max h value and assigns it the the return. This way, we only always try to find the max h value in the corresponding if block


```go
package main

func hIndex(citations []int) int {
	low, high := 0, len(citations)-1
	hidx := 0
	for low <= high {
		mid := low + (high-low)/2
		h := len(citations) - mid
		if citations[mid] >= h {
			hidx = h
			high = mid - 1
		} else {
			low = mid + 1
		}
	}
	return hidx
}
```
