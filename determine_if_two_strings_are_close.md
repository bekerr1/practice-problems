# Determine if Two Strings Are Close

## Metadata

- **Date Solved:** December 3, 2022 10:12 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/determine-if-two-strings-are-close/
- **Algorithm:** hashmap, sort
- **Type:** string

## Problem Statement

Two strings are considered close if you can attain one from the other using the following operations:


## Constraints

- 1 <= word1.length, word2.length <= 105
- word1 and word2 contain only lowercase English letters.

## Solution

- **Status:** Solved On Own

### Solution Notes

Comparing arrays in go would have simplified this since you cannot compare slices. Using a bitmask to compare existence equality was a good optimization.

Since we are only sorting at most 26 entries, the runtime using sort is O(n), since n is constant, runtime in linear.


```go
package main

import "sort"

func closeStrings(word1 string, word2 string) bool {
	if len(word1) != len(word2) {
		return false
	}

	w1Map := make(map[byte]bool, len(word1))

	w1Freq := make([]int, 26)
	w2Freq := make([]int, 26)

	for i := 0; i < len(word1); i++ {
		w1Map[word1[i]] = true

		w1Freq[word1[i]-'a']++
		w2Freq[word2[i]-'a']++
	}

	for i := 0; i < len(word2); i++ {
		delete(w1Map, word2[i])
	}
	if len(w1Map) > 0 {
		return false
	}

	sort.Slice(w1Freq, func(i, j int) bool {
		return w1Freq[i] < w1Freq[j]
	})
	sort.Slice(w2Freq, func(i, j int) bool {
		return w2Freq[i] < w2Freq[j]
	})

	for i := 0; i < len(w1Freq); i++ {
		if w1Freq[i] != w2Freq[i] {
			return false
		}
	}
	return true
}
```

### Simplified

```go
package main

import "sort"

func closeStrings(word1 string, word2 string) bool {
	counter := func(word string) (keys, vals [26]int) {
		for i := range word {
			keys[word[i]-'a'] = 1
			vals[word[i]-'a'] += 1
		}
		sort.Ints(vals[:])
		return keys, vals
	}
	keys1, vals1 := counter(word1)
	keys2, vals2 := counter(word2)
	return keys1 == keys2 && vals1 == vals2
}
```

### Best memory/runtime

```go
package main

import "sort"

func charsExistAndFrequencies(s string) (exist int32, frequencies [26]int) {
	for i := 0; i < len(s); i++ {
		cIdx := s[i] - 'a'
		exist |= 1 << cIdx
		frequencies[cIdx]++
	}
	return
}

func closeStrings(word1 string, word2 string) bool {
	if len(word1) != len(word2) {
		return false
	}
	e1, f1 := charsExistAndFrequencies(word1)
	e2, f2 := charsExistAndFrequencies(word2)
	if e1 != e2 {
		return false
	}
	sort.Ints(f1[:])
	sort.Ints(f2[:])
	return f1 == f2
}
```
