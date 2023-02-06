# Zigzag Conversion

## Metadata

- **Date Solved:** February 5, 2023 8:23 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/zigzag-conversion/description/
- **Algorithm:** brute force, math
- **Type:** string

## Problem Statement

The string "PAYPALISHIRING"

## Constraints

- 1 <= s.length <= 1000
- s consists of English letters (lower-case and upper-case), ',' and '.'.
- 1 <= numRows <= 1000

## Solution

- **Status:** Solved On Own

### Solution Notes

This problem is just meant to test raw mental strength and not meant to be any sort of test on data structures or algorithms.


```go
package main

func convert(s string, numRows int) string {
	if numRows == 1 {
		return s
	}
	var (
		zigZag = [2]int{numRows*2 - 2, 0}
		swap   = 0
		inc    = zigZag[0]
		ret    = make([]byte, len(s))
	)
	for row, seen := 0, 0; seen < len(s); row++ {
		if row == 0 || row == numRows-1 {
			inc = numRows*2 - 2
		}
		swap = 0
		for i := row; i < len(s) && seen < len(s); i += inc {
			ret[seen] = s[i]
			seen++
			if row != 0 && row != numRows-1 {
				inc = zigZag[swap%2]
				swap++
			}
		}
		zigZag[0], zigZag[1] = zigZag[0]-2, zigZag[1]+2
	}
	return string(ret)
}
```
