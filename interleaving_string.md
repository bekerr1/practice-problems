# Interleaving String

## Metadata

- **Date Solved:** July 8, 2022 11:59 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/interleaving-string/
- **Algorithm:** dynamic programming
- **Type:** string

## Problem Statement

Given strings s1, s2, and s3, find whether s3 is formed by an interleaving of s1 and s2.
An interleaving of two strings s and t is a configuration where they are divided into non-empty substrings such that:
- s = s1 + s2 + ... + sn
- t = t1 + t2 + ... + tm
- |n - m| <= 1
- The interleaving is s1 + t1 + s2 + t2 + s3 + t3 + ... or t1 + s1 + t2 + s2 + t3 + s3 + ...
Note: a + b is the concatenation of strings a and b.

## Problem


```go
package main

func isInterleave(s1 string, s2 string, s3 string) bool {
	if len(s1)+len(s2) != len(s3) {
		return false
	}

	s1Len := len(s1) + 1
	s2Len := len(s2) + 1
	dp := make([][]bool, s1Len)
	for i := range dp {
		dp[i] = make([]bool, s2Len)
	}

	for i := 0; i < s1Len; i++ {
		for j := 0; j < s2Len; j++ {
			if i == 0 && j == 0 {
				dp[i][j] = true
				continue
			}
			if i == 0 {
				dp[i][j] = dp[i][j-1] && s2[j-1] == s3[i+j-1]
			} else if j == 0 {
				dp[i][j] = dp[i-1][j] && s1[i-1] == s3[i+j-1]
			} else {
				dp[i][j] = dp[i-1][j] && s1[i-1] == s3[i+j-1] || dp[i][j-1] && s2[j-1] == s3[i+j-1]
			}
		}
	}
	return dp[s1Len-1][s2Len-1]
}
```

## Solution Statement

