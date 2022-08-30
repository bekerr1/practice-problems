# Minimum Remove to Make Valid Parentheses

## Metadata

- **Date Solved:** August 30, 2022 6:07 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/
- **Algorithm:** stack, two pass
- **Type:** string

## Problem Statement

Given a string s of '(' , ')' and lowercase English characters.
Your task is to remove the minimum number of parentheses ( '(' or ')', in any positions ) so that the resulting parentheses string is valid and return any valid string.
Formally, a parentheses string is valid if and only if:
- It is the empty string, contains only lowercase characters, or
- It can be written as AB (A concatenated with B), where A and B are valid strings, or
- It can be written as (A), where A is a valid string.

## Constraints

- 1 <= s.length <= 105
- s[i] is either'(' , ')', or lowercase English letter.

## Solution


### Two Pass

```go
package main

func minRemoveToMakeValid(s string) string {
	s = fromLeft(s)
	s = fromRight(s)
	return s
}

func fromLeft(s string) string {
	newStr := make([]byte, 0, len(s))
	nOpen, nClose := 0, 0
	for i := 0; i < len(s); i++ {
		switch s[i] {
		case '(':
			nOpen++
		case ')':
			if nOpen < nClose+1 {
				continue
			}
			nClose++
		}

		if nOpen >= nClose {
			newStr = append(newStr, s[i])
		}
	}
	return string(newStr)
}

func fromRight(s string) string {
	newStr := make([]byte, 0, len(s))
	nOpen, nClose := 0, 0
	for i := len(s) - 1; i >= 0; i-- {
		switch s[i] {
		case '(':
			if nClose < nOpen+1 {
				continue
			}
			nOpen++
		case ')':
			nClose++
		}
		if nClose >= nOpen {
			newStr = append(newStr, s[i])
		}
	}
	for i, j := 0, len(newStr)-1; i < j; i, j = i+1, j-1 {
		newStr[i], newStr[j] = newStr[j], newStr[i]
	}
	return string(newStr)
}
```

### Stack

```go
package main
```
