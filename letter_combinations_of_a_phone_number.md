# Letter Combinations of a Phone Number

## Metadata

- **Date Solved:** August 19, 2022 5:02 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/letter-combinations-of-a-phone-number/
- **Type:** string

## Problem Statement

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.
A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

## Constraints

- 0 <= digits.length <= 4
- digits[i] is a digit in the range ['2', '9'].

## Solution


```go
package main

func letterCombinations(digits string) []string {
	resSize := 1
	for i := range digits {
		resSize *= len(numMap[digits[i]])
	}
	// pre-allocate capacity so we get resize past what we need
	res := make([]string, 0, resSize)
	if digits != "" {
		letterCombinationsUtil(digits, 0, "", &res)
	}
	return res
}

var numMap = map[byte][]byte{
	'2': []byte{'a', 'b', 'c'},
	'3': []byte{'d', 'e', 'f'},
	'4': []byte{'g', 'h', 'i'},
	'5': []byte{'j', 'k', 'l'},
	'6': []byte{'m', 'n', 'o'},
	'7': []byte{'p', 'q', 'r', 's'},
	'8': []byte{'t', 'u', 'v'},
	'9': []byte{'w', 'x', 'y', 'z'},
}

func letterCombinationsUtil(digits string, curDigitIdx int, curStr string, curRes *[]string) {
	for i := curDigitIdx; i < len(digits); i++ {
		for j := 0; j < len(numMap[digits[i]]); j++ {
			curStr += string(numMap[digits[i]][j])
			letterCombinationsUtil(digits, curDigitIdx+1, curStr, curRes)
			curStr = curStr[0 : len(curStr)-1]
		}
		return
	}
	*curRes = append(*curRes, curStr)
	return
}
```

### Statement

Backtracking + recursion was key here. We needed to take every digit index and dig into the letters at each digit. One gotcha was to return after the inner for loop continue condition hits to avoid appending single characters to the result after the backtrack logic ran. This way we could continue with the last frame and allow the upper for loop to run.
