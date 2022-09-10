# The Number of Weak Characters in the Game

## Metadata

- **Date Solved:** September 10, 2022 1:31 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/the-number-of-weak-characters-in-the-game/
- **Algorithm:** greedy, sort
- **Type:** array

## Problem Statement

You are playing a game that contains multiple characters, and each of the characters has two main properties: attack and defense. You are given a 2D integer array properties where properties[i] = [attacki, defensei] represents the properties of the ith character in the game.
A character is said to be weak if any other character has both attack and defense levels strictly greater than this character's attack and defense levels. More formally, a character i is said to be weak if there exists another character j where attackj > attacki and defensej > defensei.
Return the number of weak characters.

## Constraints

- 2 <= properties.length <= 105
- properties[i].length == 2
- 1 <= attacki, defensei <= 105

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Sorting by one attribute makes it easier to compare the other attributes. Additionally, secondary sorting helps the comparison of the other attribute.


```go
package main

import "sort"

func numberOfWeakCharacters(properties [][]int) int {
	sort.Slice(properties, func(i, j int) bool {
		if properties[i][0] != properties[j][0] {
			return properties[i][0] > properties[j][0]
		}
		return properties[i][1] < properties[j][1]
	})
	ret := 0
	maxSoFar := 0
	for i := 0; i < len(properties); i++ { // 3
		if maxSoFar > properties[i][1] {
			ret++
		} else {
			maxSoFar = properties[i][1]
		}
	}
	return ret
}
```
