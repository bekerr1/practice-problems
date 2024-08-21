# Stone Game II

## Metadata

- **Date Solved:** August 20, 2024 8:21 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/stone-game-ii/description/?envType=daily-question&envId=2024-08-20
- **Algorithm:** dynamic programming, minimax, prefixsum
- **Type:** array, scenario

## Problem Statement

Alice and Bob continue their games with piles of stones.  There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].  The objective of the game is to end with the most stones. 

Alice and Bob take turns, with Alice starting first.  Initially, M = 1.

On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M.  Then, we set M = max(M, X).

The game continues until all the stones have been taken.
Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

## Constraints


- 1 <= piles.length <= 100
- 1 <= piles[i] <= 104

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I had only done one or two other minimax problems so that was the key to solve this one. I had somewhat the right idea but did not know how to apply the minimax.


```go
package main

import "math"

func stoneGameII(piles []int) int {
	memo = make(map[[2]int]int)
	maxDiff := solve(0, 1, piles)
	total := 0
	for _, p := range piles {
		total += p
	}
	return (total + maxDiff) / 2
}

var memo map[[2]int]int

func solve(idx, m int, piles []int) int {
	if idx == len(piles) {
		return 0
	}
	if v, ok := memo[[2]int{idx, m}]; ok {
		return v
	}
	ans, sum := math.MinInt, 0
	for i := 1; i < 2*m+1; i++ {
		if i+idx-1 >= len(piles) {
			break
		}
		sum += piles[idx+i-1]
		ans = max(ans, -solve(idx+i, max(m, i), piles)+sum)
	}
	memo[[2]int{idx, m}] = ans
	return ans
}
```

```go
package main

import "math"

func stoneGameII(piles []int) int {
	memo = make(map[[2]int]int)
	suffixSum := piles
	suffixSum[len(suffixSum)-1] = piles[len(piles)-1]
	for i := len(piles) - 2; i >= 0; i-- {
		suffixSum[i] += suffixSum[i+1]
	}
	return solve(0, 1, piles, suffixSum)
}

var memo map[[2]int]int

func solve(idx, m int, piles []int, suffixSum []int) int {
	if idx+(2*m) >= len(suffixSum) {
		return suffixSum[idx]
	}
	if v, ok := memo[[2]int{idx, m}]; ok {
		return v
	}
	ans := math.MaxInt
	for i := 1; i <= 2*m; i++ {
		ans = min(ans, solve(idx+i, max(m, i), piles, suffixSum))
	}
	memo[[2]int{idx, m}] = suffixSum[idx] - ans
	return memo[[2]int{idx, m}]
}
```
