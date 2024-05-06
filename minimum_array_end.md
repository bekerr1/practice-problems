# Minimum Array End

## Metadata

- **Date Solved:** May 5, 2024 10:12 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-array-end/description/
- **Algorithm:** bit manipulation
- **Type:** number theory

## Problem Statement

You are given two integers n and x. You have to construct an array of positive integers nums of size n where for every 0 <= i < n - 1, nums[i + 1] is greater than nums[i], and the result of the bitwise AND operation between all elements of nums is x.

Return the minimum possible value of nums[n - 1].

## Constraints


- 1 <= n, x <= 108

## Solution

- **Status:** Solved On Own


### Brute (TLE)

```go
package main

func minEnd(n int, x int) int64 {
	nums := make([]int64, 0, n)
	nums = append(nums, int64(x))
	for i := x + 1; len(nums) < n; i++ {
		if i&x == x {
			nums = append(nums, int64(i))
		}
	}
	return nums[len(nums)-1]
}
```

### Optimized 1

```go
package main

func minEnd(n int, x int) int64 {
	var i int64
	var numsSeen int
	x64 := int64(x)
	for i = x64; numsSeen < n; i++ {
		i |= x64
		numsSeen++
	}
	return i - 1
}
```

### Optimized 2

```cpp
long long minEnd(int n, int x) {
n--;
long long a = x, b;
for (b = 1; n > 0; b <<= 1) {
if ((b & x) == 0) {
a |= (n & 1) * b;
n >>= 1;
}
}
return a;
}
```
