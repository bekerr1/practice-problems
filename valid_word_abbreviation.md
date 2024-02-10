# Valid Word Abbreviation

## Metadata

- **Date Solved:** February 9, 2024 8:28 PM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/valid-word-abbreviation/description/
- **Algorithm:** two pointer
- **Type:** string

## Problem Statement

A string can be abbreviated by replacing any number of non-adjacent, non-emptysubstrings with their lengths. The lengths should not have leading zeros.

For example, a string such as "substitution" could be abbreviated as (but not limited to):

- "s10n" ("s ubstitutio n")
- "sub4u4" ("sub stit u tion")
- "12" ("substitution")
- "su3i1u2on" ("su bst i t u ti on")
- "substitution" (no substrings replaced)
The following are not valid abbreviations:
- "s55n" ("s ubsti tutio n", the replaced substrings are adjacent)
- "s010n" (has leading zeros)
- "s0ubstitution" (replaces an empty substring)

Given a string word and an abbreviation abbr, return whether the string matchesthe given abbreviation.

A substring is a contiguous non-empty sequence of characters within a string.

## Constraints

- 1 <= word.length <= 20
- word consists of only lowercase English letters.
- 1 <= abbr.length <= 10
- abbr consists of lowercase English letters and digits.
- All the integers in abbr will fit in a 32-bit integer.

## Solution

- **Status:** Solved On Own


### Two Pointer

```go
package main

import "strconv"

func validWordAbbreviation(word string, abbr string) bool {
	var i, j int
	for i, j = 0, 0; i < len(word) && j < len(abbr); {
		if word[i] == abbr[j] {
			i, j = i+1, j+1
			continue
		}
		var numStr []byte
		for ; j < len(abbr); j++ {
			// Leading 0
			if abbr[j] == '0' && len(numStr) == 0 {
				return false
			}
			if abbr[j]-'a' >= 0 && abbr[j]-'a' < 26 {
				break
			}
			numStr = append(numStr, abbr[j])
		}
		// word[i] != abbr[j] && we could not get a valid number
		if len(numStr) == 0 {
			return false
		}
		curNum, _ := strconv.Atoi(string(numStr))
		i += curNum
	}
	// We did not get to the end of both at the same time
	if j != len(abbr) || i != len(word) {
		return false
	}
	return true
}
```

### Regex!

```go
package main

import (
	"fmt"
	"regexp"
)

func validWordAbbreviation(word string, abbr string) bool {
	// Calculate number of digits in len(word)
	// Our regex should only match at max that many digits
	wordLen := len(word)
	digits := 0
	for wordLen > 0 {
		digits++
		wordLen /= 10
	}

	numbers := regexp.MustCompile(fmt.Sprintf(`([1-9]{1}\d{0,%v})`, digits))
	replStr := numbers.ReplaceAllString(abbr, "[a-z]{${0}}")
	abbrRe := regexp.MustCompile(fmt.Sprintf("^%v$", replStr))

	return abbrRe.MatchString(word)
}
```
