# Valid Parentheses

## Metadata

- **Date Solved:** August 28, 2022 10:53 PM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/valid-parentheses/
- **Algorithm:** stack
- **Type:** string

## Problem Statement

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

## Constraints

- 1 <= s.length <= 104
- s consists of parentheses only '()[]{}'.

## Solution


```go
package main

var openParens = map[byte]struct{}{'[': struct{}{}, '(': struct{}{}, '{': struct{}{}}
var matchingParen = map[byte]byte{']': '[', ')': '(', '}': '{'}

func isValid(s string) bool {
	stack := []byte{}
	for i := 0; i < len(s); i++ {
		if _, isOpen := openParens[s[i]]; isOpen {
			stack = append(stack, s[i])
		} else if len(stack) > 0 {
			top := stack[len(stack)-1]
			if matchingParen[s[i]] != top {
				return false
			}
			stack = stack[0 : len(stack)-1]
		} else {
			return false
		}
	}
	return len(stack) == 0
}
```
