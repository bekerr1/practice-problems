# Minimum Cost to Make All Characters Equal

## Metadata

- **Date Solved:** June 4, 2023 4:20 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-cost-to-make-all-characters-equal/description/
- **Algorithm:** dynamic programming, greedy
- **Type:** string

## Problem Statement

You are given a 0-indexed binary string s of length n on which you can apply two types of operations:
- Choose an index i and invert all characters from index 0 to index i (both inclusive), with a cost of i + 1
- Choose an index i and invert all characters from index i to index n - 1 (both inclusive), with a cost of n - i
Return the minimum cost to make all characters of the string equal.
Invert a character means if its value is '0' it becomes '1' and vice-versa.

## Constraints

- 1 <= s.length == n <= 105
- s[i] is either '0' or '1'

## Solution


### Greedy Solution

```go
package main

func minimumCost(s string) int64 {
	mid := len(s) / 2
	if len(s)%2 != 0 {
		return minimumCostUtil(s, s[mid], mid-1, mid+1)
	} else {
		return min(minimumCostUtil(s, s[mid-1], mid-1, mid), minimumCostUtil(s, s[mid], mid-1, mid))
	}
}

func minimumCostUtil(b string, target byte, left, right int) int64 {
	targetLeft := target
	targetRight := target
	var ans int64 = 0
	for l, r := left, right; l >= 0 && r < len(b); l, r = l-1, r+1 {
		if b[l] != targetLeft {
			ans += int64(l + 1)
			targetLeft = b[l]
		}
		if b[r] != targetRight {
			ans += int64(len(b) - r)
			targetRight = b[r]
		}
	}
	return ans
}

func min(x, y int64) int64 {
	if x < y {
		return x
	}
	return y
}
```

### Attempts

This attempt solved the problem but did not yield the min

```go
package main

import (
	"fmt"
	"math"
)

func minimumCost(s string) int64 {
	b := []byte(s)
	return minimumCostUtil(b)
}

func minimumCostUtil(b []byte) int64 {
	var countl, countr, count int64
	count = math.MaxInt64
	fmt.Println(string(b))
	for i, j := 1, len(b)-2; i < len(b) && j >= 0; i, j = i+1, j-1 {
		if b[i-1] != b[i] {
			cl := flipLeftOf(b, i)
			fmt.Println(cl, string(b))
			countl = cl + minimumCostUtil(b)
			count = min(count, countl)
		}
		if b[j] != b[j+1] {
			cr := flipRightOf(b, j)
			fmt.Println(cr, string(b))
			countr = cr + minimumCostUtil(b)
			count = min(count, countr)
		}
	}
	if count == math.MaxInt64 {
		return 0
	}
	fmt.Println(count, string(b))
	return count
}

func flipLeftOf(b []byte, idx int) int64 {
	cur := b[idx]
	for i := idx - 1; i >= 0; i-- {
		b[i] = cur
	}
	return int64(idx)
}
func flipRightOf(b []byte, idx int) int64 {
	cur := b[idx]
	for i := idx + 1; i < len(b); i++ {
		b[i] = cur
	}
	return int64(len(b) - idx)
}

func min(x, y int64) int64 {
	if x < y {
		return x
	}
	return y
}
```
