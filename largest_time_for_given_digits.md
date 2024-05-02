# Largest Time for Given Digits

## Metadata

- **Date Solved:** May 1, 2024 10:14 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/largest-time-for-given-digits/
- **Algorithm:** backtracking, math, recursion
- **Type:** array

## Problem Statement

Given an array arr of 4 digits, find the latest 24-hour time that can be made using each digit exactly once.

24-hour times are formatted as "HH:MM", where HH is between 00 and 23, and MM is between 00 and 59. The earliest 24-hour time is 00:00, and the latest is 23:59.

Return the latest 24-hour time in "HH:MM" format. If no valid time can be made, return an empty string.

## Constraints


- arr.length == 4
- 0 <= arr[i] <= 9

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Doing all combinations was the key here. It might not be possible to make the largest time using the greater numbers so it's required to sometimes use the second/third greater numbers to create a time. Due to this, trying combinations is best.

It's also possible to sort and return the first valid largest time. If we sort and don't find a valid time, it still may be possible to find one.  


```go
package main

import "fmt"

func largestTimeFromDigits(arr []int) string {
	maxTime = -1
	solve(arr, 0)
	if maxTime == -1 {
		return ""
	}
	return fmt.Sprintf("%.2v:%.2v", maxTime/60, maxTime%60)
}

var maxTime int

func solve(arr []int, start int) {
	if start == len(arr) {
		checkMaxTime(arr)
		return
	}

	for i := start; i < len(arr); i++ {
		swap(arr, i, start)
		solve(arr, start+1)
		swap(arr, i, start)
	}
}

func checkMaxTime(arr []int) {
	hour := arr[0]*10 + arr[1]
	minute := arr[2]*10 + arr[3]
	if hour < 24 && minute < 60 {
		time := hour*60 + minute
		if time > maxTime {
			maxTime = time
		}
	}
}

func swap(arr []int, i, j int) {
	arr[i], arr[j] = arr[j], arr[i]
}
```
