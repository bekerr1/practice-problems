# Jump Game VII

## Metadata

- **Date Solved:** July 12, 2022 7:08 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/jump-game-vii/
- **Algorithm:** bfs
- **Type:** queue

## Problem Statement

You are given a 0-indexed binary string s and two integers minJump and maxJump. In the beginning, you are standing at index 0, which is equal to '0'. You can move from index i to index j if the following conditions are fulfilled:
- i + minJump <= j <= min(i + maxJump, s.length - 1), and
- s[j] == '0'.
Return true if you can reach index s.length - 1 in s, or false otherwise.

## Problem


### Queue solution - Accepted O(n)

```go
package main

func canReach(s string, minJump int, maxJump int) bool {
	if s[len(s)-1] != '0' {
		return false
	}

	queue := make([]int, 0, len(s))
	queue = append(queue, 0)
	i := 1
	for i < len(s) && len(queue) > 0 {
		cur := queue[0]
		if cur+maxJump < i {
			queue = queue[1:]
			continue
		}

		if i >= cur+minJump && i <= min(cur+maxJump, len(s)-1) {
			if s[i] == '0' {
				if i == len(s)-1 {
					return true
				}
				queue = append(queue, i)
			}
		}
		i++
	}
	return false
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

### DP Solution - O(n)

```go
package main

func canReach(s string, minJump int, maxJump int) bool {
	if len(s) < 2 {
		return true
	}
	if s[len(s)-1] == '1' {
		return false
	}

	dp := make([]bool, len(s))
	dp[len(s)-1] = true

	for i := len(s) - 2; i >= 0; i-- {

		if s[i] == '1' {
			continue
		}

		for step := minJump; step <= maxJump; step++ {
			if step+i < len(s) && dp[step+i] == true {
				dp[i] = true
				break
			}
		}
	}
	return dp[0] == true
}
```

## Solution Statement


Originally I treated this problem as a DP problem. The issue with the way i originally approached it is, you have to look back at multiple entries in the table when setting dp[i] going accending. If we maintain a queue with the top most entry always being in range, we can see if there is ever an entry that allows us to jump to the end. We dont prioritize any position to jump from, so as long as this doesn’t matter, the implementation works. Once we say, try to jump from the min/max position, we would need extra logic (maybe a heap).
