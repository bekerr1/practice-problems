# Rotated Digits

## Metadata

- **Date Solved:** February 4, 2023 9:15 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/rotated-digits/description/
- **Algorithm:** dynamic programming, greedy
- **Type:** array

## Problem Statement

An integer x is a good if after rotating each digit individually by 180 degrees, we get a valid number that is different from x. Each digit must be rotated - we cannot choose to leave it alone.
A number is valid if each digit remains a digit after rotation. For example:
- 0, 1, and 8 rotate to themselves,
- 2 and 5 rotate to each other (in this case they are rotated in a different direction, in other words, 2 or 5 gets mirrored),
- 6 and 9 rotate to each other, and
- the rest of the numbers do not rotate to any other number and become invalid.
Given an integer n, return the number of good integers in the range [1, n].

## Constraints

- 1 <= n <= 10000

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Could use DP to keep track of the previous numbers ‘rotatability’, or just iterate through all digits of a number to determine if it can be rotated.


```go
package main

var makesGoodNumber = [10]int32{0, 0, 1, -1, -1, 1, 1, -1, 0, 1}
var goodNumbers []int32

func init() {
	initGoodNumbers()
}

func rotatedDigits(n int) int {
	return int(goodNumbers[n])
}

func initGoodNumbers() {
	goodNumbers = make([]int32, 10001)
	goodNumbers[0] = 0
	for i := 1; i <= 10000; i++ {
		n := i
		var add int32
		for n > 0 {
			modN := n % 10
			n = n / 10
			if makesGoodNumber[modN] >= 0 {
				add = max(add, makesGoodNumber[modN])
			} else {
				add = 0
				break
			}
		}
		goodNumbers[i] = goodNumbers[i-1] + add
	}
}

func max(x, y int32) int32 {
	if x > y {
		return x
	}
	return y
}
```
