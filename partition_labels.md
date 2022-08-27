# Partition Labels

## Metadata

- **Date Solved:** August 27, 2022 2:11 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/partition-labels/
- **Algorithm:** hashmap
- **Type:** string

## Problem Statement

You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part.
Note that the partition is done so that after concatenating all the parts in order, the resultant string should be s.
Return a list of integers representing the size of these parts.

## Constraints

- 1 <= s.length <= 500
- s consists of lowercase English letters.

## Solution


### Using Intervals

```go
package main
unc partitionLabels(s string) []int {
	intersections := [26][2]int{}
	for i := range intersections {
		intersections[i] = [2]int{-1, -1}
	}
	for i, c := range s {
		charRange := intersections[c-'a']
		if charRange == [2]int{-1, -1} {
			intersections[c-'a'] = [2]int{i, i}
		} else {
			intersections[c-'a'][1] = i
		}
	}

	ret := []int{}
	lastCharRange := intersections[s[0]-'a']
	for i := 1; i < len(s); i++ {
		curRange := intersections[s[i]-'a']
		if lastCharRange[1] > curRange[0] {
			lastCharRange[1] = max(curRange[1], lastCharRange[1])
		} else {
			ret = append(ret, lastCharRange[1]-lastCharRange[0]+1)
			lastCharRange = curRange
		}
	}
	ret = append(ret, lastCharRange[1]-lastCharRange[0]+1)
	return ret
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

### Tracking Max Left Idx

```go
package main

func partitionLabels(s string) []int {
	intersections := [26]int{}
	for i, c := range s {
		intersections[c-'a'] = i
	}

	ret := []int{}
	maxIntersection, last := 0, 0
	for i, c := range s {
		maxIntersection = max(maxIntersection, intersections[c-'a'])
		if maxIntersection == i {
			ret = append(ret, maxIntersection-last+1)
			last = maxIntersection + 1
		}
	}
	return ret
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
