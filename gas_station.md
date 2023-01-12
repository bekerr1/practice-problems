# Gas Station

## Metadata

- **Date Solved:** January 11, 2023 7:53 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/gas-station/description/
- **Algorithm:** greedy
- **Type:** array

## Problem Statement

There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].
You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with an empty tank at one of the gas stations.
Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique

## Constraints

- n == gas.length == cost.length
- 1 <= n <= 105
- 0 <= gas[i], cost[i] <= 104

## Solution

- **Status:** Solved On Own


```go
package main

func canCompleteCircuit(gas []int, cost []int) int {
	tank, gasSum, costSum := 0, 0, 0
	startIdx := 0

	for i := 0; i < len(gas); i++ {
		gasSum += gas[i]
		costSum += cost[i]
		tank += gas[i]
		if tank-cost[i] >= 0 {
			tank -= cost[i]
		} else {
			tank = 0
			startIdx = i + 1
		}
	}
	if gasSum >= costSum {
		return startIdx
	}
	return -1
}
```
