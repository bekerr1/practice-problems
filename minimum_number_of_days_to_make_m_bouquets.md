# Minimum Number of Days to Make m Bouquets

## Metadata

- **Date Solved:** March 1, 2024 8:24 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/description/
- **Algorithm:** binary search
- **Type:** array

## Problem Statement

You are given an integer array bloomDay, an integer m and an integer k.

You want to make m bouquets. To make a bouquet, you need to use k adjacent flowers from the garden.
The garden consists of n flowers, the ith flower will bloom in the bloomDay[i] and then can be used in exactly one bouquet.

Return the minimum number of days you need to wait to be able to make m bouquets from the garden. If it is impossible to make m bouquets return -1.

## Constraints

 
- bloomDay.length == n
- 1 <= n <= 105
- 1 <= bloomDay[i] <= 109
- 1 <= m <= 106
- 1 <= k <= n

## Solution

- **Status:** Needed Hints

### Solution Notes

I was able to solve the problem with the hints. It actually just came to me but it was a bit late. The aspect I was missing was how to find the groups of flowers that could make a valid bouquet. I was struggling how to keep track of the flowers because I had a major flaw in my logic.

I kept thinking I needed to restart the bloomDay loop for a given day, but there is no need as I would have already qualified the flowers in previous indices. It finally occurred to me that I could just keep track of how many adjacent flowers were valid and then track if I have enough for a valid bouquet. 

I should not have taken so long to come to this. Not good 😟


```go
package main

func minDays(bloomDay []int, m int, k int) int {
	if len(bloomDay) < m*k {
		return -1
	}
	l, r := 0, 1000000000
	minDays := 0
	for l <= r {
		mid := l + (r-l)/2
		if b := bouquetsAtDay(bloomDay, k, mid); b < m {
			l = mid + 1
		} else {
			minDays = mid
			r = mid - 1
		}
	}
	return minDays
}

func bouquetsAtDay(bloomDay []int, k, day int) int {
	mcount, kcount := 0, 0
	for i := 0; i < len(bloomDay); i++ {
		if bloomDay[i] <= day {
			kcount++
		} else {
			kcount = 0
		}
		if kcount == k {
			mcount++
			kcount = 0
		}
	}
	return mcount
}
```
