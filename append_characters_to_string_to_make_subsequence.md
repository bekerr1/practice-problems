# Append Characters to String to Make Subsequence

## Metadata

- **Date Solved:** March 14, 2023 10:00 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/append-characters-to-string-to-make-subsequence/description/
- **Algorithm:** brute force
- **Type:** string

## Problem Statement

You are given two strings s and t consisting of only lowercase English letters.
Return the minimum number of characters that need to be appended to the end of s so that t becomes a subsequence of s.
A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

## Constraints

- 1 <= s.length, t.length <= 105
- s and t consist only of lowercase English letters.

## Solution

- **Status:** Solved On Own


```go
package main

func appendCharacters(s string, t string) int {
	tIdx := 0
	for i := 0; i < len(s) && tIdx < len(t); i++ {
		if s[i] == t[tIdx] {
			tIdx++
		}
	}
	if tIdx == len(t) {
		return 0
	}
	return len(t) - tIdx
}
```
