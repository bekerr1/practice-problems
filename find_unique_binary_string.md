# Find Unique Binary String

## Metadata

- **Date Solved:** September 5, 2022 1:26 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-unique-binary-string/
- **Algorithm:** hashmap
- **Type:** bits, string

## Problem Statement

Given an array of strings nums containing n unique binary strings each of length n, return a binary string of length n that does not appear in nums. If there are multiple answers, you may return any of them.

## Constraints

- n == nums.length
- 1 <= n <= 16
- nums[i].length == n
- nums[i] is either '0' or '1'.
- All the strings of nums are unique.

## Solution


```go
package main

import (
	"fmt"
	"math"
	"strconv"
)

func findDifferentBinaryString(nums []string) string {
	intMap := make(map[int]struct{}, len(nums))
	for _, n := range nums {
		if i, err := strconv.ParseUint(n, 2, 64); err == nil {
			intMap[int(i)] = struct{}{}
		}
	}

	maxBit := len(nums)
	maxInt := int(math.Pow(2, float64(maxBit)))
	for i := 0; i < maxInt; i++ {
		if _, exists := intMap[i]; !exists {
			out := fmt.Sprintf("%016v", strconv.FormatUint(uint64(i), 2))
			return out[16-maxBit : 16]
		}
	}
	return ""
}
```
