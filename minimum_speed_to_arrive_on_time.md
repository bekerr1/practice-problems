# Minimum Speed to Arrive on Time

## Metadata

- **Date Solved:** August 15, 2023 5:51 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-speed-to-arrive-on-time/description/
- **Algorithm:** binary search
- **Type:** array, scenario

## Problem Statement

You are given a floating-point number hour, representing the amount of time you have to reach the office. To commute to the office, you must take n trains in sequential order. You are also given an integer array dist of length n, where dist[i] describes the distance (in kilometers) of the ith train ride.

Each train can only depart at an integer hour, so you may need to wait in between each train ride.

- For example, if the 1st train ride takes 1.5 hours, you must wait for an additional 0.5 hours before you can depart on the 2nd train ride at the 2 hour mark.

Return the minimum positive integer speed (in kilometers per hour) that all the trains must travel at for you to reach the office on time, or -1 if it is impossible to be on time.

Tests are generated such that the answer will not exceed 107 and hour will have at most two digits after the decimal point.

## Constraints

- n == dist.length
- 1 <= n <= 105
- 1 <= dist[i] <= 105
- 1 <= hour <= 109
- There will be at most two digits after the decimal point in hour.

## Solution

- **Status:** Needed Hints

### Solution Notes

I got bogged down on thinking the min number would be one that existed in the set of distances. This was because i thought atleast one number would have to be divided by itself.

The problem mentions the min positive integer, signifying it could be any number. Since it also narrows it down to two decimal places, the set of possible speeds is between 1 and 10^7, since the distances is from 1 to 10^5. 10^5/10^7 leaves two decimal places for grabs.


```go
package main

import "math"

func minSpeedOnTime(dist []int, hour float64) int {
	ret := math.MaxInt
	low, high := 0, 10000000
Outer:
	for low <= high {
		mid := low + (high-low)/2
		var curSum float64 = 0
		j := 0
		for ; j < len(dist); j++ {
			timeToTravel := float64(dist[j]) / float64(mid)
			if j < len(dist)-1 {
				curSum += math.Ceil(timeToTravel)
			} else {
				curSum += timeToTravel
			}
			if curSum > hour {
				low = mid + 1
				continue Outer
			}
		}
		ret = min(ret, mid)
		high = mid - 1
	}
	if ret == math.MaxInt {
		return -1
	}
	return ret
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```
