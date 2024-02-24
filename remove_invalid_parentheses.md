# Remove Invalid Parentheses

## Metadata

- **Date Solved:** February 24, 2024 11:06 AM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/remove-invalid-parentheses/description/
- **Algorithm:** backtracking, bfs, dfs
- **Type:** string

## Problem Statement

Given a string s that contains parentheses and letters, remove the minimum number of invalid parentheses to make the input string valid.

Return a list of unique strings that are valid with the minimum number of removals. You may return the answer in any order.

## Constraints

- 1 <= s.length <= 25
- s consists of lowercase English letters and parentheses '(' and ')'.
- There will be at most 20 parentheses in s.

## Solution

- **Status:** Needed Hints

### Solution Notes

This felt like a breakthrough solution for me as I was able to solve on my own with minimal hints.

I ended up getting the intuition of trying all combination and do a take/don't take type solution. I initially put my don't take recursive call in a for loop which was wrong. I think I need to sync up on when a recursive call should go in a for loop. I tend to default to that when I probably should do the opposite.


```go
package main

import (
	"fmt"
	"math"
)

func removeInvalidParentheses(s string) []string {
	ans = []string{}
	seen = make(map[string]bool)
	lenOfLargest, minRemoved = 0, math.MaxInt

	solve(s, "", 0, 0, 0)
	return ans
}

var ans []string
var lenOfLargest int
var minRemoved int
var seen map[string]bool

func solve(str, cur string, balance, start, removed int) {
	if balance < 0 {
		return
	}
	if removed > minRemoved {
		return
	}
	if seen[fmt.Sprintf("%v:%v", cur, start)] {
		return
	}
	seen[fmt.Sprintf("%v:%v", cur, start)] = true
	if start == len(str) {
		if balance == 0 && lenOfLargest <= len(cur) {
			if lenOfLargest < len(cur) {
				ans = []string{}
				lenOfLargest = len(cur)
				minRemoved = removed
			}
			ans = append(ans, cur)
		}
		return
	}
	solve(str, cur+string(str[start]), balance+balanceInt(str[start]), start+1, removed)
	solve(str, cur, balance, start+1, removed+1)
}

func balanceInt(b byte) int {
	switch b {
	case '(':
		return 1
	case ')':
		return -1
	}
	return 0
}
```
