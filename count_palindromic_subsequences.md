# Count Palindromic Subsequences

## Metadata

- **Date Solved:** March 22, 2023 11:42 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/count-palindromic-subsequences/description/
- **Algorithm:** dynamic programming
- **Type:** string

## Problem Statement

Given a string of digits s, return the number of palindromic subsequences of s having length 5. Since the answer may be very large, return it modulo 109 + 7.
Note:
- A string is palindromic if it reads the same forward and backward.
- A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

## Constraints

- 1 <= s.length <= 104
- s consists of digits.

## Solution

- **Status:** Looked At Discuss


### Brute Force

```go
package main

func countPalindromes(s string) int {
	if len(s) < 5 {
		return 0
	}
	ret := 0
	for i := 0; i < len(s); i++ {
		for j := i + 1; j < len(s); j++ {
			for x := j + 1; x < len(s); x++ {
				for k := x + 1; k < len(s); k++ {
					for l := k + 1; l < len(s); l++ {
						if s[i] == s[l] && s[j] == s[k] {
							ret++
						}
					}
				}
			}
		}
	}
	return ret
}
```
