# Minimum Moves to Capture The Queen

## Metadata

- **Date Solved:** January 15, 2024 5:35 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-moves-to-capture-the-queen/description/
- **Algorithm:** brute force
- **Type:** scenario

## Problem Statement

There is a 1-indexed 8 x 8 chessboard containing 3 pieces.

You are given 6 integers a, b, c, d, e, and f where:

- (a, b) denotes the position of the white rook.
- (c, d) denotes the position of the white bishop.
- (e, f) denotes the position of the black queen.

Given that you can only move the white pieces, return the minimum number of moves required to capture the black queen.

Note that:

- Rooks can move any number of squares either vertically or horizontally, but cannot jump over other pieces.
- Bishops can move any number of squares diagonally, but cannot jump over other pieces.
- A rook or a bishop can capture the queen if it is located in a square that they can move to.
- The queen does not move.

## Constraints

- 1 <= a, b, c, d, e, f <= 8
- No two pieces are on the same square.

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Tried to move/track the bishop/rook in sync. The boolean logic to get this to work was pretty complicated. I would have needed to draw diagrams and ensure all the cases were satisfied.

When I looked at discussion one much simpler solution involved moving the queen in the directions of each and then determining if either component can take without the interruption of the other. This was much simpler to code up, though it still took some guess + check on my part. 


```go
package main

var bishopMoves = [][2]int{{-1, 1}, {1, 1}, {1, -1}, {-1, -1}}
var rookMoves = [][2]int{{0, 1}, {1, 0}, {-1, 0}, {0, -1}}

func minMovesToCaptureTheQueen(a int, b int, c int, d int, e int, f int) int {
	for _, direction := range bishopMoves {
		encounteredRook, encounteredBishop := false, false
		for i := 0; i < 8; i++ {
			dirX, dirY := direction[0]+(direction[0]*i), direction[1]+(direction[1]*i)
			newE, newF := dirX+e, dirY+f
			encounteredRook = encounteredRook || a == newE && b == newF
			encounteredBishop = c == newE && d == newF
			if encounteredBishop && !encounteredRook {
				return 1
			}
		}
	}

	for _, direction := range rookMoves {
		encounteredRook, encounteredBishop := false, false
		for i := 0; i < 8; i++ {
			dirX, dirY := direction[0]+(direction[0]*i), direction[1]+(direction[1]*i)
			newE, newF := dirX+e, dirY+f
			encounteredRook = a == newE && b == newF
			encounteredBishop = encounteredBishop || c == newE && d == newF
			if encounteredRook && !encounteredBishop {
				return 1
			}
		}
	}
	return 2
}
```
