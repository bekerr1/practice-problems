# Repeated DNA Sequences

## Metadata

- **Date Solved:** November 9, 2022 10:07 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/repeated-dna-sequences/
- **Algorithm:** hashmap
- **Type:** string

## Problem Statement

The DNA sequence is composed of a series of nucleotides abbreviated as 'A', 'C', 'G', and 'T'.
- For example, "ACGAATTCCG" is a DNA sequence.
When studying DNA, it is useful to identify repeated sequences within the DNA.
Given a string s that represents a DNA sequence, return all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule. You may return the answer in any order.

## Constraints

- 1 <= s.length <= 105
- s[i] is either 'A', 'C', 'G', or 'T'.

## Solution

- **Status:** Solved On Own

### Solution Notes

The solution of hashing everything is not so bad given the count is 10. Would be good to have gone through the total memory used and everything as an exercise before chosing the right solution


### Optimal Solution

```go
package main

func findRepeatedDnaSequences(s string) []string {
	resHash := make(map[string]int)
	res := []string{}
	for i := range s {
		if i+10 > len(s) {
			break
		}
		cur := s[i : i+10]
		if resHash[cur] == 1 {
			res = append(res, cur)
		}
		resHash[cur]++
	}
	return res
}
```

### First solution (TLE)

```go
package main

func findRepeatedDnaSequences(s string) []string {
	resHash := make(map[string]bool)
	res := []string{}

	idxHash := make(map[byte][]int)
	byteS := []byte(s)

	for i, st := range byteS {
		if i+10 > len(s) {
			break
		}
		if resHash[s[i:i+10]] {
			continue
		}
		idxHash[st] = append(idxHash[st], i)
		for _, idx := range idxHash[st] {
			if idx < i && string(byteS[idx:idx+10]) == string(byteS[i:i+10]) {
				str := string(byteS[idx : idx+10])
				resHash[str] = true
				res = append(res, str)
				idxHash[st] = idxHash[st][1:]
				break
			}
		}
	}
	return res
}
```
