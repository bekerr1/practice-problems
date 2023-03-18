# Minimum Penalty for a Shop

## Metadata

- **Date Solved:** March 18, 2023 12:05 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-penalty-for-a-shop/description/
- **Algorithm:** greedy
- **Type:** string

## Problem Statement

You are given the customer visit log of a shop represented by a 0-indexed string customers consisting only of characters 'N' and 'Y':
- if the ith character is 'Y', it means that customers come at the ith hour
- whereas 'N' indicates that no customers come at the ith hour.
If the shop closes at the jth hour (0 <= j <= n), the penalty is calculated as follows:
- For every hour when the shop is open and no customers come, the penalty increases by 1.
- For every hour when the shop is closed and customers come, the penalty increases by 1.
Return the earliest hour at which the shop must be closed to incur a minimum penalty.
Note that if a shop closes at the jth hour, it means the shop is closed at the hour j.

## Constraints

- 1 <= customers.length <= 105
- customers consists only of characters 'Y' and 'N'.

## Solution

- **Status:** Solved On Own

### Solution Notes

Getting the correct idx was key. Starting at 1 and looking at previous indx with assumption the current indx will count next (if it was Y) or not count at all (if it was N).


```go
package main

func bestClosingTime(customers string) int {
	countY, countN := 0, 0
	maxValue, maxIdx := 0, 0
	for i := 1; i <= len(customers); i++ {
		switch customers[i-1] {
		case 'N':
			countN++
		case 'Y':
			countY++
		}
		total := countY - countN
		if total > maxValue {
			maxValue = total
			maxIdx = i
		}
	}
	return maxIdx
}
```
