# Lexicographically Minimum String After Removing Stars

## Metadata

- **Date Solved:** June 7, 2025 12:35 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/lexicographically-minimum-string-after-removing-stars/description/?envType=daily-question&envId=2025-06-07
- **Algorithm:** hashmap, stack
- **Type:** string

## Problem Statement

You are given a string s. It may contain any number of '*' characters. Your task is to remove all '*'characters.

While there is a '*', do the following operation:

- Delete the leftmost '*' and the smallest non-'*' character to its left. If there are several smallest characters, you can delete any of them.

Return the lexicographically smallest resulting string after removing all '*' characters.

## Constraints


- 1 <= s.length <= 105
- s consists only of lowercase English letters and '*'.
- The input is generated such that it is possible to delete all '*' characters.

## Solution

- **Status:** Solved On Own


```go
package main

func clearStars(s string) string {
	chars := [26][]int{}
	toRemove := make(map[int]bool)
	bs := []byte(s)
	for i := 0; i < len(bs); i++ {
		if s[i] == '*' {
			for c := 0; c < len(chars); c++ {
				if len(chars[c]) > 0 {
					idx := chars[c][len(chars[c])-1]
					chars[c] = chars[c][0 : len(chars[c])-1]
					toRemove[idx] = true
					toRemove[i] = true
					break
				}
			}
		} else {
			chars[s[i]-'a'] = append(chars[s[i]-'a'], i)
		}
	}

	ans := []byte{}
	for i := 0; i < len(bs); i++ {
		if toRemove[i] {
			continue
		}
		ans = append(ans, bs[i])
	}
	return string(ans)
}
```
