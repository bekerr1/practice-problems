# Find the Integer Added to Array II

## Metadata

- **Date Solved:** May 7, 2024 12:27 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-integer-added-to-array-ii/description/
- **Algorithm:** sort, two pointer
- **Type:** array

## Problem Statement

You are given two integer arrays nums1 and nums2.
From nums1 two elements have been removed, and all other elements have been increased (or decreased in the case of negative) by an integer, represented by the variable x.

As a result, nums1 becomes equal to nums2. Two arrays are considered equal when they contain the same integers with the same frequencies.

Return the minimum possible integer x that achieves this equivalence.

## Constraints


- 3 <= nums1.length <= 200
- nums2.length == nums1.length - 2
- 0 <= nums1[i], nums2[i] <= 1000
- The test cases are generated in a way that there is an integer x such that nums1 can become equal to nums2by removing two elements and adding x to each element of nums1.

## Solution

- **Status:** Needed Hints

### Solution Notes

Applying constraints by sorting helped better define an answer space.


```go
package main

import "sort"

func minimumAddedInteger(nums1 []int, nums2 []int) int {
	sort.Ints(nums1)
	sort.Ints(nums2)

	var found, ans int
	var n1low, n1high, n2low, n2high int
	ans = 1001
	for i := 0; i < 3; i++ {
		found = 1
		n1low, n1high = i, i+1
		n2low, n2high = 0, 1
		for found < len(nums2) && n1high < len(nums1) && n2high < len(nums2) {
			n1Diff := nums1[n1high] - nums1[n1low]
			n2Diff := nums2[n2high] - nums2[n2low]
			if n1Diff == n2Diff {
				found++
				n1low, n1high = n1high, n1high+1
				n2low, n2high = n2low+1, n2high+1
			} else {
				n1high++
			}
		}
		if found == len(nums2) {
			ans = min(ans, nums2[0]-nums1[i])
		}
	}
	return ans
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
