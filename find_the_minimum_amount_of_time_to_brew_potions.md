# Find the Minimum Amount of Time to Brew Potions

## Metadata

- **Date Solved:** March 23, 2025 12:04 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-minimum-amount-of-time-to-brew-potions/description/
- **Algorithm:** binary search
- **Type:** array, scenario

## Problem Statement

You are given two integer arrays, skill and mana, of length n and m, respectively.

In a laboratory, n wizards must brew m potions in order. Each potion has a mana capacity mana[j] and must pass through all the wizards sequentially to be brewed properly. The time taken by the ith wizard on the jth potion is timeij = skill[i] * mana[j].

Since the brewing process is delicate, a potion must be passed to the next wizard immediately after the current wizard completes their work. This means the timing must be synchronized so that each wizard begins working on a potion exactly when it arrives.

Return the minimum amount of time required for the potions to be brewed properly.

## Constraints


- n == skill.length
- m == mana.length
- 1 <= n, m <= 5000
- 1 <= mana[i], skill[i] <= 5000

## Solution

- **Status:** Needed Hints


```go
package main

func minTime(skill []int, mana []int) int64 {
	N, M := len(skill), len(mana)
	prevPotion := make([]int64, N)
	for potion := 0; potion < M; potion++ {
		var startTime int64
		if potion > 0 {
			// Need to find the best time between these two to start
			start, end := prevPotion[0], prevPotion[N-1]
			for start <= end {
				mid := start + (end-start)/2
				if canCompletePotion(mid, potion, prevPotion, skill, mana) {
					startTime = mid
					end = mid - 1
				} else {
					start = mid + 1
				}
			}
		}
		for wiz := 0; wiz < N; wiz++ {
			prevPotion[wiz] = startTime + int64(skill[wiz])*int64(mana[potion])
			startTime = prevPotion[wiz]
		}
	}
	return prevPotion[N-1]
}

// create the 'potionCount'th potion schedule, taking into account the previous potion schedule
// Since potions must be done sequentially, as long as we adhere to the previous potion schedule
// we should be good across all schedules relative to previous.
func canCompletePotion(startTime int64, potionCount int, prevPotion []int64, skill, mana []int) bool {
	for wizard := 0; wizard < len(skill); wizard++ {
		doneTime := startTime + int64(skill[wizard])*int64(mana[potionCount])
		// If the done time for the previous potion is greater than this one
		// it means we cannot hand off this potion to the next wizard in a sequential
		// manner.
		if prevPotion[wizard] > startTime {
			return false
		}
		startTime = doneTime
	}
	// As long as all potions are done after the previous, we are ok.
	return true
}
```
