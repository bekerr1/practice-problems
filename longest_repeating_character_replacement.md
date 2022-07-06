# Longest Repeating Character Replacement

## Metadata

- **Date Solved:** July 5, 2022 8:18 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/longest-repeating-character-replacement/
- **Algorithm:** slidingwindow
- **Type:** array, hashmap

## Problem Statement

You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.
Return the length of the longest substring containing the same letter you can get after performing the above operations.

## Problem


```go
package main

type Frequency [26]int

func (f Frequency) MaxEntry() int {
	max := 0
	for _, n := range f {
		if n > max {
			max = n
		}
	}
	return max
}

func characterReplacement(s string, k int) int {
	freq := new(Frequency)
	max := 0
	for start, end := 0, 0; end < len(s); end++ {
		endIdx := s[end] - 'A'
		freq[endIdx]++

		maxEntry := freq.MaxEntry()
		if (end-start+1)-maxEntry > k {
			startIdx := s[start] - 'A'
			freq[startIdx]--
			start++
		}
		if max < end-start+1 {
			max = end - start + 1
		}
	}
	return max
}
```

## Solution Statement


The goal here was to calculate ‘len(substr) - maxFrequency ≥ k’. To calculate the len substring, we keep a sliding window and do end - start + 1. To calculate the max frequency, we keep a frequency map of all characters seen. The thoughts around the formula is, As long as the difference between the whole substring and the most frequent characters is less than or equal to ‘k’, we can ensure that we would be able to replace those characters with the max frequency character. When the formula is not satisfied, we need to shrink the sliding window.
