# Pow(x, n)

## Metadata

- **Date Solved:** August 12, 2023 4:58 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/powx-n/description/
- **Algorithm:** math, recursion
- **Type:** number theory

## Problem Statement

Implement http://www.cplusplus.com/reference/valarray/pow/, which calculates x raised to the power n (i.e., xn).

## Constraints

- -100.0 < x < 100.0
- -231 <= n <= 231-1
- n is an integer.
- Either x is not zero or n > 0.
- -104 <= xn <= 104

## Solution

- **Status:** Solved On Own


```go
package main

func myPow(x float64, n int) float64 {
	negative := n < 0
	if negative {
		n = -n
	}
	ret := powUtil(x, n)
	if negative {
		return 1 / ret
	}
	return ret
}

func powUtil(base float64, n int) float64 {
	if n == 0 {
		return 1
	}
	if n%2 == 0 {
		return powUtil(base*base, n/2)
	} else {
		return base * powUtil(base, n-1)
	}
}
```
