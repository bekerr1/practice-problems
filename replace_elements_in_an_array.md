# Replace Elements in an Array

## Metadata

- **Date Solved:** September 4, 2022 11:12 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/replace-elements-in-an-array/
- **Algorithm:** hashmap
- **Type:** array

## Problem Statement

You are given a 0-indexed array nums that consists of n distinct positive integers. Apply m operations to this array, where in the ith operation you replace the number operations[i][0] with operations[i][1].
It is guaranteed that in the ith operation:
- operations[i][0] exists in nums.
- operations[i][1] does not exist in nums.
Return the array obtained after applying all the operations.

## Constraints

- n == nums.length
- m == operations.length
- 1 <= n, m <= 105
- All the values of nums are distinct.
- operations[i].length == 2
- 1 <= nums[i], operations[i][0], operations[i][1] <= 106
- operations[i][0] will exist in nums when applying the ith operation.
- operations[i][1] will not exist in nums when applying the ith operation.

## Solution


```go
package main

func arrayChange(nums []int, operations [][]int) []int {
	idxMap := make(map[int]int, len(nums))
	for i, n := range nums {
		idxMap[n] = i
	}
	for _, op := range operations {
		idx := idxMap[op[0]]
		nums[idx] = op[1]
		delete(idxMap, op[0])
		idxMap[op[1]] = idx
	}
	return nums
}
```

### Statement

Originally thought to go with a sorting/binary search solution but didn’t take into account the ‘nums’ array ordering is changed with every operation. Additionally, the hash map implementation does good with O(m) anyway so runtime ends up being O(n+m) which is fine. Should have started with the hash map first then tried to optimize.
