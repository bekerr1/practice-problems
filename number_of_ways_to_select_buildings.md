# Number of Ways to Select Buildings

## Metadata

- **Date Solved:** March 19, 2025 9:48 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/number-of-ways-to-select-buildings/description/?envType=study-plan-v2&envId=amazon-spring-23-high-frequency
- **Algorithm:** dynamic programming, prefixsum
- **Type:** string

## Problem Statement

You are given a 0-indexed binary string s which represents the types of buildings along a street where:

- s[i] = '0' denotes that the ith building is an office and
- s[i] = '1' denotes that the ith building is a restaurant.

As a city official, you would like to select 3 buildings for random inspection. However, to ensure variety, no two consecutive buildings out of the selected buildings can be of the same type.

- For example, given s = "001101", we cannot select the 1st, 3rd, and 5th buildings as that would form "011" which is not allowed due to having two consecutive buildings of the same type.

Return the number of valid ways to select 3 buildings.

## Constraints


- 3 <= s.length <= 105
- s[i] is either '0' or '1'.

## Solution

- **Status:** Looked At Discuss


```go
package main

func numberOfWays(s string) int64 {
	total0, total1 := count01(s)
	count0, count1 := 0, 0

	var ans int64
	for i := range s {
		if s[i] == '0' {
			ans += int64(count1 * (total1 - count1))
			count0++
		} else {
			ans += int64(count0 * (total0 - count0))
			count1++
		}
	}
	return ans
}

func count01(s string) (int, int) {
	var zero, one int
	for i := range s {
		if s[i] == '0' {
			zero++
		} else {
			one++
		}
	}
	return zero, one
}
```
