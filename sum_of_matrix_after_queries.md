# Sum of Matrix After Queries

## Metadata

- **Date Solved:** June 6, 2023 6:40 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/sum-of-matrix-after-queries/description/
- **Algorithm:** brute force
- **Type:** array, matrix

## Problem Statement

You are given an integer n and a 0-indexed 2D array queries where queries[i] = [typei, indexi, vali].
Initially, there is a 0-indexed n x n matrix filled with 0's. For each query, you must apply one of the following changes:
- if typei == 0, set the values in the row with indexi to vali, overwriting any previous values.
- if typei == 1, set the values in the column with indexi to vali, overwriting any previous values.
Return the sum of integers in the matrix after all queries are applied.

## Constraints

- 1 <= n <= 104
- 1 <= queries.length <= 5 * 104
- queries[i].length == 3
- 0 <= typei <= 1
- 0 <= indexi < n
- 0 <= vali <= 105

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Keeping the whole matrix in memory was too much. I had trouble coming up with a solution to the problem iterating through the queries front to back because the new queries were overlaid onto the old. Instead, iterating back to front we could track the number of reamining rows/cols and the reserved ones and calculate a running sum.


### Simple Approach

```go
package main

func matrixSumQueries(n int, queries [][]int) int64 {
	mat := make([][]int, n)
	for i := range mat {
		mat[i] = make([]int, n)
	}

	var ret int64
	for _, q := range queries {
		switch q[0] {
		case 0:
			row := q[1]
			for col := 0; col < n; col++ {
				ret -= int64(mat[row][col])
				mat[row][col] = q[2]
				ret += int64(q[2])
			}
		case 1:
			col := q[1]
			for row := 0; row < n; row++ {
				ret -= int64(mat[row][col])
				mat[row][col] = q[2]
				ret += int64(q[2])
			}
		}
	}
	return ret
}
```

### Optimized

```go
package main

const (
	Row    = 0
	Column = 1
)

func matrixSumQueries(n int, queries [][]int) int64 {
	rowsLeft, colsLeft := n, n
	typeConsumed := [2][50001]bool{}
	var ret int64

	for i := len(queries) - 1; i >= 0; i-- {
		t := queries[i][0]
		idx := queries[i][1]
		val := queries[i][2]

		if !typeConsumed[t][idx] {
			typeConsumed[t][idx] = true
			switch t {
			case Row:
				colsLeft--
				ret += int64(val * rowsLeft)
			case Column:
				rowsLeft--
				ret += int64(val * colsLeft)
			}
		}
	}
	return ret
}
```
