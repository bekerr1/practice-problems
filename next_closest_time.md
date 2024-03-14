# Next Closest Time

## Metadata

- **Date Solved:** March 14, 2024 7:25 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/next-closest-time/
- **Algorithm:** brute force
- **Type:** string

## Problem Statement

Given a time represented in the format "HH:MM", form the next closest time by reusing the current digits. There is no limit on how many times a digit can be reused.

You may assume the given input string is always valid. For example, "01:34", "12:09" are all valid. "1:34", "12:9" are all invalid.

## Constraints


- time.length == 5
- time is a valid time in the form "HH:MM".
- 0 <= HH < 24
- 0 <= MM < 60

## Solution

- **Status:** Solved On Own


```go
package main

import (
	"sort"
	"strconv"
	"strings"
)

func nextClosestTime(time string) string {
	digits := []int{}
	for i := range time {
		if time[i] == ':' {
			continue
		}
		digits = append(digits, int(time[i]-'0'))
	}
	sort.Ints(digits)
	for i := len(time) - 1; i >= 0; i-- {
		if time[i] == ':' {
			continue
		}
		ans := time[0:i]
		for j := range digits {
			if digits[j] <= int(time[i]-'0') {
				continue
			}
			ans += strconv.Itoa(digits[j])
			break
		}
		ans += time[i+1 : len(time)]
		if isValid(ans) {
			return ans
		}
		time = time[0:i] + strconv.Itoa(digits[0]) + time[i+1:len(time)]
	}
	return time
}

func isValid(time string) bool {
	split := strings.Split(time, ":")
	return len(time) == 5 && split[0] <= "23" && split[1] <= "59"
}
```
