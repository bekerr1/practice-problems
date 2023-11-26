# Letter Tile Possibilities

## Metadata

- **Date Solved:** November 26, 2023 11:38 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/letter-tile-possibilities/
- **Algorithm:** bitmask, dfs, hashmap, recursion
- **Type:** string

## Problem Statement

You have n  tiles, where each tile has one letter tiles[i] printed on it.

Return the number of possible non-empty sequences of letters you can make using the letters printed on those tiles.

## Constraints

- 1 <= tiles.length <= 7
- tiles consists of uppercase English letters.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Had a feeling bit mask could be used but wanted to try to solve without it. Was going on the right path at first in terms of using a recursive for-loop to get all combinations. I could not figure out how to account for index's already used (which is what the bit mask would have done). Could not come up with a solution for more than an hour. Had to look at discussions to see.

Possible follow-up to this might have been to use counting instead of keeping track of all combinations, which one of the liked solutions did.


```go
package main

var have map[string]bool

func numTilePossibilities(tiles string) int {
	have = make(map[string]bool)
	solve(tiles, "", 0)
	return len(have) - 1
}

func solve(tiles string, cur string, bs uint8) {
	have[cur] = true
	for i := 0; i < len(tiles); i++ {
		if bs&(1<<i) == 0 {
			solve(tiles, cur+tiles[i:i+1], bs|(1<<i))
		}
	}
}
```
