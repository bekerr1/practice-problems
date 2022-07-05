# Longest Substring Without Repeating Characters

## Metadata

- **Date Solved:** July 5, 2022 12:47 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/longest-substring-without-repeating-characters/
- **Algorithm:** slidingwindow
- **Type:** blind75, hashmap, string

## Problem Statement

Given a string s, find the length of the longest substring
 without repeating characters.

## Constraints


- 0 <= s.length <= 5 * 104
- s consists of English letters, digits, symbols and spaces.

## Problem

- **Status:** Solved On Own


```go
package main

func lengthOfLongestSubstring(s string) int {
	start, end := 0, 0
	chars := make(map[byte]int)
	ans := 0
	for end < len(s) {
		if chars[s[end]-'a'] == 0 {
			chars[s[end]-'a']++
			end++
		} else {
			for start < end && chars[s[end]-'a'] > 0 {
				chars[s[start]-'a']--
				start++
			}
		}
		ans = max(ans, end-start)
	}
	return ans
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

```go
package main

func lengthOfLongestSubstring(s string) int {
	freq := make(map[byte]bool)
	max := 0
	start, end := 0, 0
	for end < len(s) {
		for freq[s[end]] {
			freq[s[start]] = false
			start++
		}

		freq[s[end]] = true
		end++

		if end-start > max {
			max = end - start
		}
	}
	return max
}
```
