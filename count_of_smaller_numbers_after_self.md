# Count of Smaller Numbers After Self

## Metadata

- **Date Solved:** January 13, 2024 10:31 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/count-of-smaller-numbers-after-self/description/
- **Algorithm:** binary index tree
- **Type:** array

## Problem Statement

Given an integer array nums, return an integer array counts where counts[i] is the number of smaller elements to the right of nums[i].

## Constraints

- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 1000
- 1 <= m * n <= 105
- grid[i][j] is either 0 or 1.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Another “range query” problem that I started trying to think in terms of monotonic stack and DP instead of BIT like other range problems. The 'range query' aspect comes from having to find “the range of numbers that meet some condition after a given number”.

One thing to note with BIT, the index should start at 1. That is where the 20002 comes from here.  


### Brute Force (TLE)

```go
package main

func countSmaller(nums []int) []int {
	ans := make([]int, len(nums))
	for i := 0; i < len(nums); i++ {
		cur := nums[i]
		for j := i + 1; j < len(nums); j++ {
			if cur > nums[j] {
				ans[i]++
			}
		}
	}
	return ans
}
```

### BIT

```go
package main

type BIT [20002]int

func (b *BIT) insert(k int) {
	for ; k < len(b); k += (k & (-k)) {
		b[k]++
	}
}

func (b *BIT) query(k int) int {
	res := 0
	for ; k > 0; k -= (k & (-k)) {
		res += b[k]
	}
	return res
}

func countSmaller(nums []int) []int {
	bit := BIT{}
	ans := make([]int, len(nums))
	for i := len(nums) - 1; i >= 0; i-- {
		num := nums[i] + 10001
		count := bit.query(num - 1)
		ans[i] = count
		bit.insert(num)
	}
	return ans
}
```
