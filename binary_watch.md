# Binary Watch

## Metadata

- **Date Solved:** August 23, 2022 9:01 AM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/binary-watch/
- **Algorithm:** brute force
- **Type:** bits

## Problem Statement

A binary watch has 4 LEDs on the top to represent the hours (0-11), and 6 LEDs on the bottom to represent the minutes (0-59). Each LED represents a zero or one, with the least significant bit on the right.
Given an integer turnedOn which represents the number of LEDs that are currently on (ignoring the PM), return all possible times the watch could represent. You may return the answer in any order.
The hour must not contain a leading zero.
- For example, "01:00" is not valid. It should be "1:00".
The minute must be consist of two digits and may contain a leading zero.
- For example, "10:2" is not valid. It should be "10:02".

## Solution


```go
package main

import (
	"fmt"
	"math/bits"
)

func readBinaryWatch(turnedOn int) []string {
	res := []string{}
	var m, h uint
	for h = 0; h < 12; h++ {
		for m = 0; m < 60; m++ {
			if bits.OnesCount(h)+bits.OnesCount(m) == turnedOn {
				res = append(res, fmt.Sprintf("%d:%02d", h, m))
			}
		}
	}
	return res
}
```

### Statement

I tried to approach this with some intelligence at first and could not find a way to solve it. Looked at the solution and realized just pure brute force to calculate all possible outcomes and see which ones matched the constraints was enough.
