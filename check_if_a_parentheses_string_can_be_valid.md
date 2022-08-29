# Check if a Parentheses String Can Be Valid

## Metadata

- **Date Solved:** August 28, 2022 10:17 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/check-if-a-parentheses-string-can-be-valid/
- **Algorithm:** two pass
- **Type:** string

## Problem Statement

A parentheses string is a non-empty string consisting only of '(' and ')'. It is valid if any of the following conditions is true:
- It is ().
- It can be written as AB (A concatenated with B), where A and B are valid parentheses strings.
- It can be written as (A), where A is a valid parentheses string.
You are given a parentheses string s and a string locked, both of length n. locked is a binary string consisting only of '0's and '1's. For each index i of locked,
- If locked[i] is '1', you cannot change s[i].
- But if locked[i] is '0', you can change s[i] to either '(' or ')'.
Return true if you can make s a valid parentheses string. Otherwise, return false.

## Constraints

- n == s.length == locked.length
- 1 <= n <= 105
- s[i] is either '(' or ')'.
- locked[i] is either '0' or '1'.

## Solution


```go
package main

func canBeValid(s string, locked string) bool {
	if len(s)%2 != 0 {
		return false
	}

	balanced, flippable := 0, 0
	for i := 0; i < len(s); i++ {
		if locked[i] == '0' {
			flippable++
		} else if s[i] == '(' {
			balanced++
		} else {
			balanced--
		}

		if balanced+flippable < 0 {
			return false
		}
	}
	if balanced > flippable {
		return false
	}

	balanced, flippable = 0, 0
	for i := len(s) - 1; i >= 0; i-- {
		if locked[i] == '0' {
			flippable++
		} else if s[i] == ')' {
			balanced++
		} else {
			balanced--
		}

		if balanced+flippable < 0 {
			return false
		}
	}
	if balanced > flippable {
		return false
	}

	return true
}
```

### Statement

First pass was to address if we have enough indicies to flip to match ‘)’ from left to right and second pass was to address if we have enough indicies to flip to match ‘(’ from right to left
