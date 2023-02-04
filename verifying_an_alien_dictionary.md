# Verifying an Alien Dictionary

## Metadata

- **Date Solved:** February 4, 2023 11:24 AM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/verifying-an-alien-dictionary/description/
- **Algorithm:** sort
- **Type:** array, hashmap

## Problem Statement

In an alien language, surprisingly, they also use English lowercase letters, but possibly in a different order. The order of the alphabet is some permutation of lowercase letters.
Given a sequence of words written in the alien language, and the order of the alphabet, return true if and only if the given words are sorted lexicographically in this alien language.

## Constraints

- 1 <= words.length <= 100
- 1 <= words[i].length <= 20
- order.length == 26
- All characters in words[i] and order are English lowercase letters.

## Solution

- **Status:** Solved On Own


### Complexity

| Time | Space |
| --- | --- |
| O(M) where M is the total number
of characters. Since there are 26 unique characters, we need not consider comparing the letter-order. | O(1). This is because there are always 26 characters in our alpha map. |

### Code

```go
package main

func isAlienSorted(words []string, order string) bool {
	alienAlpha := [26]int{}
	for i := range order {
		alienAlpha[order[i]-'a'] = i
	}

	for i := 1; i < len(words); i++ {
		prev := words[i-1]
		cur := words[i]
		if len(prev) > len(cur) && cur == prev[0:len(cur)] {
			return false
		}
		var j int
		for j = range prev {
			posp := alienAlpha[prev[j]-'a']
			posc := alienAlpha[cur[j]-'a']
			if posp > posc {
				return false
			} else if posp == posc {
				continue
			}
			break
		}
	}
	return true
}
```
