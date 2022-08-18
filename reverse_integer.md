# Reverse Integer

## Metadata

- **Date Solved:** August 18, 2022 2:27 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/reverse-integer/
- **Algorithm:** math
- **Type:** math

## Problem Statement

Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-231, 231 - 1], then return 0.
Assume the environment does not allow you to store 64-bit integers (signed or unsigned).

## Solution


```go
package main

func reverse(x int) int {
	var reversedNum int32
	var prevRevNum int32
	var curDigit int32

	isNegative := x < 0
	if isNegative {
		x = -x
	}

	for x > 0 {
		curDigit = int32(x % 10)
		reversedNum = reversedNum*10 + curDigit
		if (reversedNum-curDigit)/10 != prevRevNum {
			return 0
		}
		prevRevNum = reversedNum
		x /= 10
	}

	if isNegative {
		reversedNum = -reversedNum
	}
	return int(reversedNum)
}
```

### Statement

This problem was very hard for me and i could not solve it on my own. The math behind reversing a number should be easier for me to grok.

234 / 10 = 23.4

234 % 10 = 4, then 3, then 2

0 * 10 + 4 = 4, then 43, then 432

Detecting if the number overflowed was difficult too. If we take the inverse of

revNum * 10 + curDigit, that is (revNum - curDigit) / 10, this should equal the prevRevNum. If we overflowed, this would not be the case.
