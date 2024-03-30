# Minimum Deletions to Make String K-Special

## Metadata

- **Date Solved:** March 30, 2024 11:53 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-deletions-to-make-string-k-special/description/
- **Algorithm:** hashmap, math
- **Type:** string

## Problem Statement

You are given a string word and an integer k.
We consider word to be k-special if |freq(word[i]) - freq(word[j])| <= k for all indices i and j in the string.

Here, freq(x) denotes the frequency of the character x in word, and |y| denotes the absolute value of y.

Return the minimum number of characters you need to delete to make word k-special.

## Constraints


- 1 <= word.length <= 105
- 0 <= k <= 105
- word consists only of lowercase English letters.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Intuition behind this is a bit tricky. To find a solution, attempting to identify some frequency value such that all frequencies larger than this one would be k-special and all frequencies smaller would be eliminated, such that the final answer results in the min.

Initially, I tried to actually figure out what values were k special but really it was more simple than that.


```go
package main

import "math"

func minimumDeletions(word string, k int) int {
	freq := [26]int{}
	for i := range word {
		freq[word[i]-'a']++
	}
	ans := math.MaxInt
	for _, fa := range freq {
		tmp := 0
		for _, fb := range freq {
			if fb < fa {
				tmp += fb
			} else {
				tmp += max(fb-(fa+k), 0)
			}
		}
		ans = min(ans, tmp)
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
