# Maximize Score After N Operations

## Metadata

- **Date Solved:** May 18, 2023 11:21 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/maximize-score-after-n-operations/description/
- **Algorithm:** bitmask, dynamic programming
- **Type:** array

## Problem Statement

You are given nums, an array of positive integers of size 2 * n. You must perform n operations on this array.
In the ith operation (1-indexed), you will:
- Choose two elements, x and y.
- Receive a score of i * gcd(x, y).
- Remove x and y from nums.
Return the maximum score you can receive after performing n operations.
The function gcd(x, y) is the greatest common divisor of x and y.

## Constraints

- 1 <= n <= 7
- nums.length == 2 * n
- 1 <= nums[i] <= 106

## Solution

- **Status:** Looked At Discuss


### Brute Force (TLE)

```go
package main

func maxScore(nums []int) int {
	gcdMemo = make(map[[2]int]int)
	removed = make(map[int]bool)
	res := maxScoreUtil(nums, 1, 0)
	return res
}

func maxScoreUtil(nums []int, nth int, mask int) int {
	if len(removed) == len(nums) {
		return 0
	}
	ret := 0
	for i := 0; i < len(nums); i++ {
		if removed[i] {
			continue
		}
		for j := i + 1; j < len(nums); j++ {
			if removed[j] {
				continue
			}

			gcd := _gcd(nums[i], nums[j])
			removed[i] = true
			removed[j] = true
			ret = max(ret, nth*gcd+maxScoreUtil(nums, nth+1, mask))
			delete(removed, i)
			delete(removed, j)
		}
	}
	return ret
}

func _gcd(x, y int) int {
	for x != y && (x != 1 || y != 1) {
		if g, ok := gcdMemo.Get(x, y); ok {
			return g
		}
		if x < y {
			y -= x
		} else {
			x -= y
		}
	}
	if x == 1 || y == 1 {
		return 1
	}
	// just return x since x && y are ==
	return x
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### DP

```go
package main

var memo map[int]int

func maxScore(nums []int) int {
	gcdMemo = make(map[[2]int]int)
	memo = make(map[int]int)
	res := maxScoreUtil(nums, 1, 0<<16)
	return res
}

func maxScoreUtil(nums []int, nth int, mask int) int {
	if mask^((1<<len(nums))-1) == 0 {
		return 0
	}
	if v, ok := memo[mask]; ok {
		return v
	}

	ret := 0
	for i := 0; i < len(nums); i++ {
		if mask>>i&1 == 1 {
			continue
		}
		for j := i + 1; j < len(nums); j++ {
			if mask>>j&1 == 1 {
				continue
			}
			gcd := _gcd(nums[i], nums[j])
			ret = max(ret, nth*gcd+maxScoreUtil(nums, nth+1, mask|(1<<i)|(1<<j)))
		}
	}
	memo[mask] = ret
	return memo[mask]
}

func _gcd(x, y int) int {
	for x != y && (x != 1 || y != 1) {
		if x < y {
			y -= x
		} else {
			x -= y
		}
	}
	if x == 1 || y == 1 {
		return 1
	}
	// just return x since x && y are ==
	return x
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### DP + GCD Optimized

```go
package main

type GCDCache map[[2]int]int

func (c GCDCache) Set(x, y int, gcd int) {
	if x < y {
		c[[2]int{x, y}] = gcd
	}
	c[[2]int{y, x}] = gcd
}
func (c GCDCache) Get(x, y int) (int, bool) {
	if x < y {
		g, ok := c[[2]int{x, y}]
		return g, ok
	}
	g, ok := c[[2]int{y, x}]
	return g, ok
}

var gcdMemo GCDCache
var memo map[int]int

func maxScore(nums []int) int {
	gcdMemo = make(map[[2]int]int)
	memo = make(map[int]int)
	res := maxScoreUtil(nums, 1, 0<<16)
	return res
}

func maxScoreUtil(nums []int, nth int, mask int) int {
	if mask^((1<<len(nums))-1) == 0 {
		return 0
	}
	if v, ok := memo[mask]; ok {
		return v
	}

	ret := 0
	for i := 0; i < len(nums); i++ {
		if mask>>i&1 == 1 {
			continue
		}
		for j := i + 1; j < len(nums); j++ {
			if mask>>j&1 == 1 {
				continue
			}
			gcd := 0
			if g, ok := gcdMemo.Get(nums[i], nums[j]); ok {
				gcd = g
			} else {
				gcd = _gcd(nums[i], nums[j])
				gcdMemo.Set(nums[i], nums[j], gcd)
			}
			ret = max(ret, nth*gcd+maxScoreUtil(nums, nth+1, mask|(1<<i)|(1<<j)))
		}
	}
	memo[mask] = ret
	return memo[mask]
}

func _gcd(x, y int) int {
	for x != y && (x != 1 || y != 1) {
		if g, ok := gcdMemo.Get(x, y); ok {
			return g
		}
		if x < y {
			y -= x
		} else {
			x -= y
		}
	}
	if x == 1 || y == 1 {
		return 1
	}
	// just return x since x && y are ==
	return x
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
