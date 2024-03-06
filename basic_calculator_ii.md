# Basic Calculator II

## Metadata

- **Date Solved:** March 6, 2024 12:30 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/basic-calculator-ii/description/
- **Algorithm:** math, stack
- **Type:** string

## Problem Statement

Given a string s which represents an expression, evaluate this expression and return its value. 

The integer division should truncate toward zero.
You may assume that the given expression is always valid. All intermediate results will be in the range of [-231, 231 - 1].

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as eval().

## Constraints


- 1 <= s.length <= 3 * 105
- s consists of integers and operators ('+', '-', '*', '/') separated by some number of spaces.
- s represents a valid expression.
- All the integers in the expression are non-negative integers in the range [0, 231 - 1].
- The answer is guaranteed to fit in a 32-bit integer.

## Solution

- **Status:** Looked At Discuss


```go
package main

func calculate(s string) int {
	numStack := []int{}
	var num, oper int
	for i := 0; i <= len(s); i++ {
		if i < len(s) && s[i] == ' ' {
			continue
		}
		if i < len(s) && isDigit(s[i]) {
			num = (num * 10) + int(s[i]-'0')
			continue
		} else if len(numStack) > 0 {
			switch s[oper] {
			case '+':
				numStack = append(numStack, num)
			case '-':
				numStack = append(numStack, -num)
			case '*':
				numStack[len(numStack)-1] *= num
			case '/':
				numStack[len(numStack)-1] /= num
			}
		} else {
			numStack = append(numStack, num)
		}
		num, oper = 0, i
	}
	ans := 0
	for i := range numStack {
		ans += numStack[i]
	}
	return ans
}

func isDigit(b byte) bool {
	return b-'0' >= 0 && b-'0' <= 9
}
```
