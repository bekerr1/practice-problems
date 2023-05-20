# Count Ways To Build Good Strings

## Metadata

- **Date Solved:** May 20, 2023 11:58 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/count-ways-to-build-good-strings/description/
- **Algorithm:** dynamic programming
- **Type:** string

## Problem Statement

Given the integers zero, one, low, and high, we can construct a string by starting with an empty string, and then at each step perform either of the following:
- Append the character '0' zero times.
- Append the character '1' one times.
This can be performed any number of times.
A good string is a string constructed by the above process having a length between low and high (inclusive).
Return the number of different good strings that can be constructed satisfying these properties. Since the answer can be large, return it modulo 109 + 7.

## Constraints

- 1 <= low <= high <= 105
- 1 <= zero, one <= low

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Needed to constrain the problem to the bounds of low:high instead of building up bit by bit from zero/one upward.


### OOM Solution

```go
package main

var zeros []byte
var ones []byte
var memo map[[2]int]int

const mod = 1000000007

func countGoodStrings(low int, high int, zero int, one int) int {
	zeros = make([]byte, zero)
	for i := 0; i < zero; i++ {
		zeros[i] = '0'
	}
	ones = make([]byte, one)
	for i := 0; i < one; i++ {
		ones[i] = '1'
	}
	memo = make(map[[2]int]int)
	return countGoodStringsUtil(low, high, 0, 0)
}

func countGoodStringsUtil(low, high int, zc, oc int) int {
	if (len(zeros)*zc)+(len(ones)*oc) > high {
		return 0
	}
	if v, ok := memo[[2]int{zc, oc}]; ok {
		return v
	}
	var ret int
	if (len(zeros)*zc)+(len(ones)*oc) >= low {
		ret++
	}
	ret = (ret + countGoodStringsUtil(low, high, zc+1, oc)) % mod
	ret = (ret + countGoodStringsUtil(low, high, zc, oc+1)) % mod
	memo[[2]int{zc, oc}] = ret
	return ret
}
```

### Correct

```go
package main

var memo map[int]int

const mod = 1000000007

func countGoodStrings(low int, high int, zero int, one int) int {
	memo = make(map[int]int)
	return countGoodStringsUtil(0, low, high, zero, one)
}

func countGoodStringsUtil(length, low, high int, zero, one int) int {
	if length > high {
		return 0
	}
	if v, ok := memo[length]; ok {
		return v
	}
	var ret int
	if length >= low {
		ret = 1
	}
	ret = (ret + countGoodStringsUtil(length+zero, low, high, zero, one)) % mod
	ret = (ret + countGoodStringsUtil(length+one, low, high, zero, one)) % mod
	memo[length] = ret
	return ret
}
```
