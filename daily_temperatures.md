# Daily Temperatures

## Metadata

- **Date Solved:** December 21, 2022 11:26 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/daily-temperatures/description/
- **Algorithm:** monotonic stack
- **Type:** array

## Problem Statement

Given an array of integers temperatures
 represents the daily temperatures, return an array
 answersuch thatanswer[i]is the number of days you have to wait after theithday to get a warmer temperature. If there is no future day for which this is possible, keep answer[i] == 0instead.

## Constraints

- 1 <= temperatures.length <= 105
- 30 <= temperatures[i] <= 100

## Solution

- **Status:** Needed Hints

### Solution Notes

Should have thought of monotonic stack first. Questions like this are common with that data structure.


```go
package main

func dailyTemperatures(temperatures []int) []int {
	monoStack := []int{}
	ret := make([]int, len(temperatures))
	for i := 0; i < len(temperatures); i++ {
		topIdx := len(monoStack) - 1
		for len(monoStack) > 0 && temperatures[monoStack[topIdx]] < temperatures[i] {
			ret[monoStack[topIdx]] = i - monoStack[topIdx]
			monoStack = monoStack[0 : len(monoStack)-1]
			topIdx = len(monoStack) - 1
		}
		monoStack = append(monoStack, i)
	}
	return ret
}
```
