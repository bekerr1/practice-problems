# Remove Digit From Number to Maximize Result

## Metadata

- **Date Solved:** September 7, 2022 9:27 AM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/remove-digit-from-number-to-maximize-result/
- **Algorithm:** brute force
- **Type:** string

## Problem Statement

You are given a string number representing a positive integer and a character digit. Return the resulting string after removing exactly one occurrence of digit from number such that the value of the resulting string in decimal form is maximized. The test cases are generated such that digit occurs at least once in number.

## Constraints

- 2 <= number.length <= 100
- number consists of digits from '1' to '9'.
- digit is a digit from '1' to '9'.
- digit occurs at least once in number.

## Solution

- **Status:** Solved On Own


```go
package main

func removeDigit(number string, digit byte) string {
	ret := []byte{}
	var i int
	var lastDigitIdx int
	var deletedDigit bool
	for i = 0; i < len(number) && !deletedDigit; i++ {
		if number[i] == digit {
			lastDigitIdx = i
			if i+1 < len(number) && number[i] < number[i+1] {
				deletedDigit = true
				continue
			}
		}
		ret = append(ret, number[i])
	}
	if !deletedDigit {
		ret = append(ret[:lastDigitIdx], ret[lastDigitIdx+1:]...)
	} else if i < len(number) {
		ret = append(ret, number[i:len(number)]...)
	}
	return string(ret)
}
```
