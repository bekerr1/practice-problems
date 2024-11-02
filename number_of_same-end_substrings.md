# Number of Same-End Substrings

## Metadata

- **Date Solved:** November 2, 2024 2:30 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/number-of-same-end-substrings/description/?envType=weekly-question&envId=2024-11-01
- **Algorithm:** brute force, math
- **Type:** string

## Problem Statement

You are given a 0-indexed string s, and a 2D array of integers queries, where queries[i] = [li, ri] indicates a substring of s starting from the index li and ending at the index ri (both inclusive), i.e. s[li..ri].

Return an array ans where ans[i] is the number of same-end substrings of queries[i].

A 0-indexed string t of length n is called same-end if it has the same character at both of its ends, i.e., t[0] == t[n - 1].

A substring is a contiguous non-empty sequence of characters within a string.

## Constraints


- 2 <= s.length <= 3 * 104
- s consists only of lowercase English letters.
- 1 <= queries.length <= 3 * 104
- queries[i] = [li, ri]
- 0 <= li <= ri < s.length

## Solution

- **Status:** Needed Hints


```go
package main

func sameEndSubstringCount(s string, queries [][]int) []int {
	ans := make([]int, len(queries))
	for i := 0; i < len(queries); i++ {
		qst, qend := queries[i][0], queries[i][1]
		ans[i] += qend - qst + 1
		charCount := [26]int{}
		for j := qst; j <= qend; j++ {
			charCount[s[j]-'a']++
		}
		for j := 0; j < 26; j++ {
			ans[i] += (charCount[j] * (charCount[j] - 1) / 2)
		}
	}
	return ans
}
```
