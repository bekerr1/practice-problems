# Count Submatrices With All Ones

## Metadata

- **Date Solved:** September 17, 2023 11:01 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/count-submatrices-with-all-ones/
- **Algorithm:** brute force, math, monotonic stack
- **Type:** array, matrix

## Problem Statement

Given an m x n binary matrix mat, return the number of submatrices that have all ones.

## Constraints

- 1 <= m, n <= 150
- mat[i][j] is either 0 or 1.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

This problem was TOUGH. It was not obvious how to solve it at all. What helped was writing out what you would expect at each row and trying to form a pattern. This lead to the solution i had to look up. For example:

1 1 0 1 = 1 + 2 + 0 + 1
1 1 1 0 = 2 + 4 + 1 + 0
1 1 0 0 = 3 + 6 + 0 + 0

As you can see, columns that are part of a larger matrix are increasing in a pattern. With this, we can iterate and count each row n times. If we count row 3, 3 times, we would get ‘3 + 6’ for that section of the matrix.

Another tricky aspect was only counting single cells when we start at that row (as we descend down the matrix).


### Brute Force

```go
package main

func numSubmat(mat [][]int) int {
	ans := 0
	ones := make([]int, len(mat[0]))
	for i := 0; i < len(mat[0]); i++ {
		ones[i] = 1
	}
	for i := 0; i < len(mat); i++ {
		row := make([]int, 0, len(ones))
		row = append(row, ones...)
		for j := i; j < len(mat); j++ {
			for k := 0; k < len(mat[j]); k++ {
				row[k] &= mat[j][k]
			}
			ans += solveRow(row)
		}
	}
	return ans
}

func solveRow(row []int) int {
	ans := 0
	length := 0
	for i := 0; i < len(row); i++ {
		if row[i] == 1 {
			length++
		} else {
			length = 0
		}
		ans += length
	}
	return ans
}
```

### Monotonic Stack

```go
package main
```
