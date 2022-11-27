# Sum of Subarray Minimums

## Metadata

- **Date Solved:** November 26, 2022 10:28 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/sum-of-subarray-minimums/
- **Algorithm:** monotonic stack
- **Type:** array

## Problem Statement

Given an array of integers arr, find the sum of min(b), where b ranges over every (contiguous) subarray of arr. Since the answer may be large, return the answer modulo 109 + 7

## Constraints

- 1 <= arr.length <= 3 * 104
- 1 <= arr[i] <= 3 * 104

## Solution

- **Status:** Looked At Discuss

### Solution Notes

understanding how to calculate the number of subarrays with x as min is key. No looking at the problem from a dp/memoization lens and more from a O(n) lens helps


```go
package main

var mod = 1000000007

func sumSubarrayMins(arr []int) int {
	st := []int{-1}
	var ans int
	for i := 0; i <= len(arr); i++ {
		for len(st) > 1 && (i == len(arr) || arr[st[len(st)-1]] > arr[i]) {
			mid := st[len(st)-1]
			st = st[0 : len(st)-1]
			left := st[len(st)-1]
			right := i

			count := ((mid - left) * (right - mid)) % mod
			ans = (ans + (arr[mid] * count)) % mod
		}
		st = append(st, i)
	}
	return ans
}
```
