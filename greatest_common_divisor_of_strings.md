# Greatest Common Divisor of Strings

## Metadata

- **Date Solved:** February 1, 2023 9:02 AM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/greatest-common-divisor-of-strings/description/
- **Algorithm:** math
- **Type:** string

## Problem Statement

For two strings s and t, we say "t divides s" if and only if s = t + ... + t (i.e., t is concatenated with itself one or more times).
Given two strings str1 and str2, return the largest string x such that x divides both str1 and str2.

## Constraints

- 1 <= str1.length, str2.length <= 1000
- str1 and str2 consist of English uppercase letters

## Solution

- **Status:** Solved On Own

### Solution Notes

Asking myself, what information would i need to know to help solve this problem? The answer was greatest common denominator between the two. Once you know that if the substring of the greatest common denominator doesn’t divide the string, return “”


### Complexity

| Time | Space |
| --- | --- |
| O(N) where N is len of largest string | O(G) where G is greatest common denominator between the two strings |

### Code

```go
package main

func gcdOfStrings(str1 string, str2 string) string {
	gcd := 0
	for i := 1; i <= len(str1) && i <= len(str2); i++ {
		if len(str1)%i == 0 && len(str2)%i == 0 {
			gcd = i
		}
	}
	gcd1 := str1[0:gcd]
	gcd2 := str2[0:gcd]
	if gcd1 != gcd2 {
		return ""
	}
	for i := gcd; i < len(str1) || i < len(str2); i += gcd {
		if i < len(str1) {
			if gcd1 != str1[i:i+gcd] {
				return ""
			}
		}
		if i < len(str2) {
			if gcd2 != str2[i:i+gcd] {
				return ""
			}
		}
	}
	return gcd1
}
```
