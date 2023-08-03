# Reverse Words in a String

## Metadata

- **Date Solved:** August 3, 2023 6:20 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/reverse-words-in-a-string/description/
- **Algorithm:** two pointer
- **Type:** string

## Problem Statement

Given an input string s, reverse the order of the words.
A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.
Return a string of the words in reverse order concatenated by a single space.

Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces

## Constraints

- 1 <= s.length <= 104
- s contains English letters (upper-case and lower-case), digits, and spaces ' '.
- There is at least one word in s.

## Solution

- **Status:** Solved On Own

### Solution Notes

Was able to come up with a crude solution after doing a lot of print statements and debugging. Had the right idea off the bat, but handling the details was tricky for me here. Need to get better at coming up with elegant solutions easier and faster.


```go
package main

func reverseWords(s string) string {
	// strings are immutable in Go, create byte slice
	bs := []byte(s)

	// Reverse everything to start
	reverse(bs, 0, len(bs)-1)

	var i, j, lastj int

	// Find first non space idx and strip
	for i = 0; i < len(bs) && bs[i] == ' '; i++ {
		continue
	}
	bs = bs[i:len(bs)]

	for i = 0; i < len(bs); i = j + 1 {
		// Move to next space
		for j = i; j < len(bs) && bs[j] != ' '; j++ {
			lastj = j
			continue
		}

		// Reverse from last char to next space - 1
		reverse(bs, i, j-1)

		// take all bytes including a single space
		front := bs[0 : j+1]

		// start at the single space and move to next non-space idx
		for i = j; i < len(bs) && bs[i] == ' '; i++ {
			continue
		}

		// take all places from next non-space until end
		back := bs[i:len(bs)]

		// update byte slice
		bs = append(front, back...)
	}

	// Finally, strip off chars from last non space idx
	bs = bs[0 : lastj+1]
	return string(bs)
}

func reverse(b []byte, st, end int) {
	for ; st < end; st, end = st+1, end-1 {
		b[st], b[end] = b[end], b[st]
	}
}
```
