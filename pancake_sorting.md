# Pancake Sorting

## Metadata

- **Date Solved:** January 23, 2024 11:12 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/pancake-sorting/
- **Algorithm:** greedy, sort
- **Type:** array

## Problem Statement

Given an array of integers arr, sort the array by performing a series of pancake flips.

In one pancake flip we do the following steps:

- Choose an integer k where 1 <= k <= arr.length.
- Reverse the sub-array arr[0...k-1] (0-indexed).

For example, if arr = [3,2,1,4] and we performed a pancake flip choosing k = 3, we reverse the sub-array [3,2,1], so arr = [1,2,3,4] after the pancake flip at k = 3.

Return an array of the k-values corresponding to a sequence of pancake flips that sort arr. Any valid answer that sorts the array within 10 * arr.length flips will be judged as correct.

## Constraints

- 1 <= arr.length <= 100
- 1 <= arr[i] <= arr.length
- All integers in arr are unique (i.e. arr is a permutation of the integers from 1 to arr.length).

## Solution

- **Status:** Solved On Own

### Solution Notes

Intuition here was to work towards moving the last element to its place. To do this, we find what would be the last element during this iteration, move it to first, then flip it from first till its correct position. For every iteration, we decrement what the 'last' element would be, as our current last is in the right place.


```go
package main

func pancakeSort(arr []int) []int {
	ans := []int{}
	for i := len(arr) - 1; i >= 0; i-- {
		for j := 0; j < len(arr); j++ {
			if arr[j] == i+1 {
				ans = append(ans, j+1)
				flip(0, j, arr)
				break
			}
		}
		ans = append(ans, i+1)
		flip(0, i, arr)
	}
	return ans
}

func flip(i, j int, arr []int) {
	for i < j {
		arr[i], arr[j] = arr[j], arr[i]
		i, j = i+1, j-1
	}
}
```
