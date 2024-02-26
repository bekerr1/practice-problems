# Sum of Scores of Built Strings

## Metadata

- **Date Solved:** February 25, 2024 9:16 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/sum-of-scores-of-built-strings/description/
- **Algorithm:** z-algorithm
- **Type:** string

## Problem Statement

You are building a string s of length n one character at a time, prepending each new character to the front of the string. The strings are labeled from 1 to n, where the string with length i is labeled si.
- For example, for s = "abaca", s1 == "a", s2 == "ca", s3 == "aca", etc.

The score of si is the length of the longest common prefix between si and sn(Note that s == sn).
Given the final string s, return the sum of the score of every si.

## Constraints


- 1 <= s.length <= 105
- s consists of lowercase English letters.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Z algorithm that I didn't know about. Had to look it up and learn it. Rolling hash solutions as too complicated.


```go
package main

func sumScores(s string) int64 {
	left, right := 0, 0
	z := make([]int, len(s))
	z[0] = len(s)
	for i := 1; i < len(s); i++ {
		if i < right {
			z[i] = min(right-i, z[i-left])
		}
		for i+z[i] < len(s) && s[z[i]] == s[i+z[i]] {
			z[i]++
		}
		if i+z[i] > right {
			left = i
			right = i + z[i]
		}
	}
	var ans int64
	for i := range z {
		ans += int64(z[i])
	}
	return ans
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
