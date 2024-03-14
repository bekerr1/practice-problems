# K Empty Slots

## Metadata

- **Date Solved:** March 14, 2024 7:31 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/k-empty-slots/
- **Algorithm:** binary index tree
- **Type:** array

## Problem Statement

You have n bulbs in a row numbered from 1 to n. Initially, all the bulbs are turned off. We turn on exactly one bulb every day until all bulbs are on after n days.
You are given an array bulbs of length n where bulbs[i] = x means that on the (i+1)th day, we will turn on the bulb at position x where i is 0-indexed and x is 1-indexed.

Given an integer k, return the minimum day number such that there exists two turned on bulbs that have exactly k bulbs between them that are all turned off. If there isn't such day, return -1.

## Constraints


- n == bulbs.length
- 1 <= n <= 2 * 104
- 1 <= bulbs[i] <= n
- bulbs is a permutation of numbers from 1 to n.
- 0 <= k <= 2 * 104

## Solution

- **Status:** Solved On Own


### Brute Force (TLE)

```go
package main

func kEmptySlots(bulbs []int, k int) int {
	bulbStatus := make([]int, len(bulbs))
	for i := 0; i < len(bulbs); i++ {
		bulbStatus[bulbs[i]-1] = 1
		low, high := -1, -1
		for j := 0; j < len(bulbStatus); j++ {
			if bulbStatus[j] == 1 {
				switch {
				case low < 0:
					low = j
				case high < 0:
					high = j
				}
				if low >= 0 && high >= 0 {
					if high-low-1 == k {
						return i + 1
					} else {
						low = high
						high = -1
					}
				}
			}
		}
	}
	return -1
}
```

### BIT

```go
package main

type BIT [20002]int

func (b *BIT) add(key int) {
	for i := key; i > 0; i -= (i & (-i)) {
		b[i]++
	}
}

func (b *BIT) query(key int) int {
	res := 0
	for i := key; i < len(b); i += (i & (-i)) {
		res += b[i]
	}
	return res
}

func kEmptySlots(bulbs []int, k int) int {
	bit := BIT{}
	bulbStatus := make([]int, len(bulbs)+1)

	for i := 0; i < len(bulbs); i++ {
		for _, idx := range []int{bulbs[i] - k - 1, bulbs[i] + k + 1} {
			if idx > 0 && idx <= len(bulbs) && bulbStatus[idx] > 0 {
				low, high := min(idx, bulbs[i]), max(idx, bulbs[i])
				inbetween := bit.query(low+1) - bit.query(high)
				if inbetween == 0 {
					return i + 1
				}
			}
		}
		bit.add(bulbs[i])
		bulbStatus[bulbs[i]] = 1
	}
	return -1
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
