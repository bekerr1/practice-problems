# Candy

## Metadata

- **Date Solved:** April 30, 2024 9:36 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/candy/description/?envType=study-plan-v2&envId=top-interview-150
- **Algorithm:** brute force, greedy
- **Type:** array

## Problem Statement

There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.
You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return the minimum number of candies you need to have to distribute the candies to the children.

## Constraints


- n == ratings.length
- 1 <= n <= 2 * 104
- 0 <= ratings[i] <= 2 * 104

## Solution

- **Status:** Solved On Own


```go
package main

func candy(ratings []int) int {
	candyCount := make([]int, len(ratings))
	for i, j := 1, len(candyCount)-2; i < len(candyCount) && j >= 0; i, j = i+1, j-1 {
		if ratings[i] > ratings[i-1] && candyCount[i] <= candyCount[i-1] {
			candyCount[i] = candyCount[i-1] + 1
		}
		if ratings[j] > ratings[j+1] && candyCount[j] <= candyCount[j+1] {
			candyCount[j] = candyCount[j+1] + 1
		}
	}
	ret := 0
	for i := 0; i < len(candyCount); i++ {
		ret += candyCount[i] + 1
	}
	return ret
}
```
