# Generate Parentheses

## Metadata

- **Date Solved:** August 24, 2022 8:06 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/generate-parentheses/
- **Algorithm:** backtracking, recursion
- **Type:** string

## Problem Statement

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

## Constraints

- 1 <= n <= 8

## Solution


```go
package main

func generateParenthesis(n int) []string {
	ret := []string{}
	generateParenthesisUtil(n, 0, 0, "", &ret)
	return ret
}

func generateParenthesisUtil(n int, nOpen, nClose int, cur string, ret *[]string) {
	if nOpen == n && nClose == n {
		*ret = append(*ret, cur)
		return
	}
	if nOpen < n {
		generateParenthesisUtil(n, nOpen+1, nClose, cur+"(", ret)
	}
	if nClose < nOpen {
		generateParenthesisUtil(n, nOpen, nClose+1, cur+")", ret)
	}
}
```
