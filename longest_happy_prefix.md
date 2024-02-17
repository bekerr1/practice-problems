# Longest Happy Prefix

## Metadata

- **Date Solved:** February 17, 2024 12:50 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/longest-happy-prefix/description/
- **Algorithm:** hashmap, rolling hash
- **Type:** string

## Problem Statement

A string is called a happy prefix if is a non-empty prefix which is also a suffix (excluding itself).

Given a string s, return the longest happy prefix of s. Return an empty string ""if no such prefix exists.

## Constraints

- 1 <= s.length <= 105
- s contains only lowercase English letters.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

https://cp-algorithms.com/string/string-hashing.html


### Hash Map (TLE)

Frequent hash map updates result in bad performance leading to TLE.

```go
package main

func longestPrefix(s string) string {
	suffixMap := make(map[string]bool)
	ans := ""
	for h, i := 0, len(s)-1; h <= len(s) && i >= 0; h, i = h+1, i-1 {
		suffixMap[s[i:len(s)]] = true
		if suffixMap[s[0:h]] {
			ans = s[0:h]
		}
	}
	return ans
}
```

### String Comparison

O(n*n)

```go
package main

func longestPrefix(s string) string {
	ans := ""
	for i := 1; i < len(s); i++ {
		prefix, suffix := s[0:i], s[len(s)-i:len(s)]
		if prefix == suffix {
			ans = prefix
		}
	}
	return ans
}
```

### Rolling Hash

O(n+n) = O(n)

```go
package main

func longestPrefix(s string) string {
	ans := ""
	h1, h2, pow, mod := 0, 0, 1, 1000000007
	for i := 0; i < len(s)-1; i++ {
		l, h := int(s[i]-'a'+1), int(s[len(s)-i-1]-'a'+1)
		// 31^i + s[i]
		h1 = (h1*31 + l) % mod
		// 31 * s[len(s)-i-1] ^ i
		h2 = (h2 + pow*h) % mod
		if h1 == h2 {
			ans = s[0 : i+1]
		}
		pow = (pow * 31) % mod
	}
	return ans
}
```
