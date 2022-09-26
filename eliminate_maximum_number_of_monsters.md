# Eliminate Maximum Number of Monsters

## Metadata

- **Date Solved:** September 26, 2022 8:48 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/eliminate-maximum-number-of-monsters/
- **Algorithm:** greedy, sort
- **Type:** array

## Problem Statement

You are playing a video game where you are defending your city from a group of n monsters. You are given a 0-indexed integer array dist of size n, where dist[i] is the initial distance in kilometers of the ith monster from the city.
The monsters walk toward the city at a constant speed. The speed of each monster is given to you in an integer array speed of size n, where speed[i] is the speed of the ith monster in kilometers per minute.
You have a weapon that, once fully charged, can eliminate a single monster. However, the weapon takes one minute to charge.The weapon is fully charged at the very start.
You lose when any monster reaches your city. If a monster reaches the city at the exact moment the weapon is fully charged, it counts as a loss, and the game ends before you can use your weapon.
Return the maximum number of monsters that you can eliminate before you lose, or n if you can eliminate all the monsters before they reach the city.

## Constraints

- n == dist.length == speed.length
- 1 <= n <= 105
- 1 <= dist[i], speed[i] <= 105

## Solution

- **Status:** Solved On Own

### Solution Notes

Simple solution with sorting dist/speed and maintaining how many minutes have passed


```go
package main

import "sort"

func eliminateMaximum(dist []int, speed []int) int {

	steps := make([]float64, len(dist))
	for i := range dist {
		steps[i] = float64(dist[i]) / float64(speed[i])
	}

	sort.Slice(steps, func(i, j int) bool {
		return steps[i] < steps[j]
	})

	eliminated := 0
	minutes := 0
	for i := 0; i < len(steps); i++ {
		if steps[i]-float64(minutes) <= float64(0) {
			break
		}
		eliminated++
		minutes++
	}
	return eliminated
}
```
