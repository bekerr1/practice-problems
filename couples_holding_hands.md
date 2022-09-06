# Couples Holding Hands

## Metadata

- **Date Solved:** September 6, 2022 4:11 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/couples-holding-hands/
- **Algorithm:** greedy, number theory, swap
- **Type:** array

## Problem Statement

There are n couples sitting in 2n seats arranged in a row and want to hold hands. The people and seats are represented by an integer array row where row[i] is the ID of the person sitting in the ith seat. The couples are numbered in order, the first couple being (0, 1), the second couple being (2, 3), and so on with the last couple being (2n - 2, 2n - 1).
Return the minimum number of swaps so that every couple is sitting side by side. A swap consists of choosing any two people, then they stand up and switch seats.

## Constraints

- 2n == row.length
- 2 <= n <= 30
- n is even.
- 0 <= row[i] < 2n
- All the elements of row are unique.

## Solution

- **Status:** Looked At Discuss


### Hash Map

```go
package main

func minSwapsCouples(row []int) int {
	idxMap := make(map[int]int, len(row))
	for i, r := range row {
		idxMap[r] = i
	}
	var ret int
	for i := 0; i < len(row); i = i + 2 {
		var desiredForCur int
		curAtIdx := row[i]
		if curAtIdx%2 == 0 {
			desiredForCur = curAtIdx + 1
		} else {
			desiredForCur = curAtIdx - 1
		}
		if row[i+1] != desiredForCur {
			idxOfDesired := idxMap[desiredForCur]
			row[i+1], row[idxOfDesired] = row[idxOfDesired], row[i+1]
			idxMap[row[i+1]] = i + 1
			idxMap[row[idxOfDesired]] = idxOfDesired
			ret++
		}
	}
	return ret
}
```

### Statement

This problem was much simpler than I anticipated. The intuition behind it is to iterate for i = 0; i += 2 and check if couple[i] and couple[i+1] are what we desire. If that is not the case, we simply swap out the current for our desired. In this way, after we iterate through the whole row, we will have done the least amount of swaps required.
