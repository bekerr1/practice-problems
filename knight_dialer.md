# Knight Dialer

## Metadata

- **Date Solved:** May 23, 2023 8:13 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/knight-dialer/
- **Algorithm:** dynamic programming
- **Type:** scenario

## Problem Statement

The chess knight has a unique movement, it may move two squares vertically and one square horizontally, or two squares horizontally and one square vertically (with both forming the shape of an L).

We have a chess knight and a phone pad as shown below, the knight can only stand on a numeric cell (i.e. blue cell).

Given an integer n, return how many distinct phone numbers of length n we can dial.
You are allowed to place the knight on any numeric cell initially and then you should perform n - 1 jumps to dial a number of length n. All jumps should be valid knight jumps.
As the answer may be very large, return the answer modulo 109 + 7.

## Constraints

- 1 <= n <= 5000

## Solution

- **Status:** Solved On Own


### Top Down

```go
package main

var keyMap = [10][]int{
	{4, 6}, {6, 8}, {7, 9}, {4, 8}, {0, 3, 9},
	{}, {0, 1, 7}, {2, 6}, {1, 3}, {2, 4},
}
var memo [5000][10]int
var mod int = 1000000007

func knightDialer(n int) int {
	ret := 0
	memo = [5000][10]int{}
	for i := 0; i < 10; i++ {
		ret = (ret + knightDialerUtil(n, i, 1)) % mod
	}
	return ret
}

func knightDialerUtil(n int, cur, count int) int {
	if n == count {
		return 1
	}
	if v := memo[count][cur]; v > 0 {
		return v
	}
	ret := 0
	for _, next := range keyMap[cur] {
		ret = (ret + knightDialerUtil(n, next, count+1)) % mod
	}
	memo[count][cur] = ret
	return ret
}
```

### Bottom Up

```go
package main
```

### Whiteboard

```go
package main

func knightDialer(n int) int {

}
```
