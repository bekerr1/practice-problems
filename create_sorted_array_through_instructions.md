# Create Sorted Array through Instructions

## Metadata

- **Date Solved:** May 10, 2023 11:40 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/create-sorted-array-through-instructions/description/
- **Algorithm:** fenwick tree
- **Type:** array

## Problem Statement

Given an integer array instructions, you are asked to create a sorted array from the elements in instructions. You start with an empty container nums. For each element from left to right in instructions, insert it into nums. The cost of each insertion is the minimum of the following:
- The number of elements currently in nums that are strictly less than instructions[i].
- The number of elements currently in nums that are strictly greater than instructions[i].
For example, if inserting element 3 into nums = [1,2,3,5], the cost of insertion is min(2, 1) (elements 1 and 2 are less than 3, element 5 is greater than 3) and nums will become [1,2,3,3,5].
Return the total cost to insert all elements from instructions into nums. Since the answer may be large, return it modulo 109 + 7

## Constraints

- 1 <= instructions.length <= 105
- 1 <= instructions[i] <= 105

## Solution

- **Status:** Looked At Discuss


```go
package main

type BIT []int

func (b BIT) get(x int) int {
	sum := 0
	for ; x > 0; x -= (x & -x) {
		sum += b[x]
	}
	return sum
}
func (b BIT) add(x int) {
	for ; x < maxValue; x += (x & -x) {
		b[x]++
	}
}

const mod = 1000000007
const maxValue = 100001

func createSortedArray(instructions []int) int {
	bit := make(BIT, maxValue)
	ret := 0
	for i := 0; i < len(instructions); i++ {
		ret = (ret + min(bit.get(instructions[i]-1), i-bit.get(instructions[i]))) % mod
		bit.add(instructions[i])
	}
	return ret
}

func min(x, y int) int {
	if x > y {
		return y
	}
	return x
}
```
