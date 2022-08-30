# Valid Parenthesis String

## Metadata

- **Date Solved:** August 30, 2022 3:21 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/valid-parenthesis-string/
- **Algorithm:** greedy, two pass
- **Type:** string

## Problem Statement

Given a string s containing only three types of characters: '(', ')' and '*', return true if s is valid.
The following rules define a valid string:
- Any left parenthesis '(' must have a corresponding right parenthesis ')'.
- Any right parenthesis ')' must have a corresponding left parenthesis '('.
- Left parenthesis '(' must go before the corresponding right parenthesis ')'.
- '*' could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string "".

## Constraints

- 1 <= s.length <= 100
- s[i] is '(', ')' or '*'.

## Solution


```go
package main

func checkValidString(s string) bool {
	return validFromFront(s) && validFromBack(s)
}

func validFromFront(s string) bool {
	nOpen, nClose, wild := 0, 0, 0
	for i := 0; i < len(s); i++ {
		switch s[i] {
		case '(':
			nOpen++
		case ')':
			nClose++
			if nOpen < nClose && nClose-nOpen > wild {
				return false
			}
		case '*':
			wild++
		}
	}
	return true
}

func validFromBack(s string) bool {
	nOpen, nClose, wild := 0, 0, 0
	for i := len(s) - 1; i >= 0; i-- {
		switch s[i] {
		case '(':
			nOpen++
			if nClose < nOpen && nOpen-nClose > wild {
				return false
			}
		case ')':
			nClose++
		case '*':
			wild++
		}
	}
	return true
}
```
