# Longest Palindromic Substring

## Metadata

- **Date Solved:** February 10, 2022 9:31 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/longest-palindromic-substring/
- **Algorithm:** slidingwindow
- **Type:** blind75, string

## Problem Statement

Given a string s, return the longest palindromic substring in s.

```go
package main

func longestPalindrome(s string) string {
	var right, left int
	var maxString string

	for i := 0; i < len(s); i++ {
		left, right = i-1, i
		for right < len(s) && s[i] == s[right] {
			right++
		}
		for left >= 0 && right < len(s) && s[left] == s[right] {
			left--
			right++
		}
		if len(maxString) < len(s[left+1:right]) {
			maxString = s[left+1 : right]
		}
	}
	return maxString
}
```

### Solution Notes
