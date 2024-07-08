# Odd Even Jump

## Metadata

- **Date Solved:** July 7, 2024 10:04 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/odd-even-jump/description/
- **Algorithm:** binary search, dynamic programming, sort
- **Type:** array

## Problem Statement

You are given an integer array arr. From some starting index, you can make a series of jumps. The (1st, 3rd, 5th, ...) jumps in the series are called odd-numbered jumps, and the (2nd, 4th, 6th, ...) jumps in the series are called even-numbered jumps. Note that the jumps are numbered, not the indices.

You may jump forward from index i to index j (with i < j) in the following way:

- During odd-numbered jumps (i.e., jumps 1, 3, 5, ...), you jump to the index j such that arr[i] <= arr[j] and arr[j] is the smallest possible value. If there are multiple such indices j, you can only jump to the smallest such index j.

- During even-numbered jumps (i.e., jumps 2, 4, 6, ...), you jump to the index j such that arr[i] >= arr[j] and arr[j] is the largest possible value. If there are multiple such indices j, you can only jump to the smallest such index j.

- It may be the case that for some index i, there are no legal jumps.
A starting index is good if, starting from that index, you can reach the end of the array (index arr.length - 1) by jumping some number of times (possibly 0 or more than once).

Return the number of good starting indices.


## Constraints


- 1 <= arr.length <= 2 * 104
- 0 <= arr[i] < 105

## Solution

- **Status:** Looked At Discuss


```go
package main

import (
	"math"
	"sort"
)

func oddEvenJumps(arr []int) int {
	N := len(arr)
	idx := []int{N, N - 1, N + 1}
	arr = append(arr, math.MinInt, math.MaxInt)
	ans := 1
	oddJumps, evenJumps := make([]int, N+2), make([]int, N+2)
	oddJumps[N-1], evenJumps[N-1] = 1, 1

	for i := N - 2; i >= 0; i-- {
		j := sort.Search(len(idx), func(k int) bool {
			return arr[idx[k]] >= arr[i]
		})
		if arr[idx[j]] == arr[i] {
			// The current element in arr (i) is the
			// same as the largest. We move the idx for this
			// element from j to i to meet the "smallest such idx"
			// reqirement.
			oddJumps[i], evenJumps[i] = evenJumps[idx[j]], oddJumps[idx[j]]
			idx[j] = i
		} else {
			// The next largest element is different than current.
			// That means arr[idx[j]] is the next largest and arr[idx[j-1]]
			// is the last smallest of arr[i].
			// Calculate odd jumps for this idx by looking at even jumps
			// from the larger idx as odd has to jump to a higher number
			// Calculate even jumps for this idx by looking at odd jumps
			// from the smaller idx as even has to jump to lower number
			oddJumps[i], evenJumps[i] = evenJumps[idx[j]], oddJumps[idx[j-1]]

			idx = append(idx, 0)
			copy(idx[j+1:], idx[j:])
			idx[j] = i
		}
		ans += oddJumps[i]
	}
	return ans
}
```
