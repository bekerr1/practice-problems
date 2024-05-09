# Minimum Length of Anagram Concatenation

## Metadata

- **Date Solved:** May 8, 2024 9:49 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-length-of-anagram-concatenation/description/
- **Algorithm:** brute force, math
- **Type:** string

## Problem Statement

You are given a string s, which is known to be a concatenation of anagrams of some string t.

Return the minimum possible length of the string t.

An anagram is formed by rearranging the letters of a string. For example, "aab", "aba", and, "baa" are anagrams of "aab".

## Constraints


- 1 <= s.length <= 105
- s consist only of lowercase English letters.

## Solution

- **Status:** Needed Hints


```go
package main

func minAnagramLength(s string) int {
	for i := 0; i < len(s); i++ {
		if len(s)%(i+1) != 0 {
			continue
		}
		charCount := [26]int{}
		for ii := 0; ii <= i; ii++ {
			charCount[s[ii]-'a']++
		}
		size := i + 1
		anagrams := false
		for j := i + 1; j+size <= len(s); j = j + size {
			anagrams = isAnagram(charCount, s[j:j+size])
			if !anagrams {
				i = j - 1
				break
			}
		}
		if anagrams {
			return size
		}
	}
	return len(s)
}

func isAnagram(count [26]int, s string) bool {
	for i := range s {
		if count[s[i]-'a'] == 0 {
			return false
		}
		count[s[i]-'a']--
	}
	return true
}
```
