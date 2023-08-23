# Find the Longest Equal Subarray

## Metadata

- **Date Solved:** August 23, 2023 9:23 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-the-longest-equal-subarray/description/
- **Algorithm:** hashmap, slidingwindow
- **Type:** array

## Problem Statement

You are given a 0-indexed integer array nums and an integer k.

A subarray is called equal if all of its elements are equal. Note that the empty subarray is an equal subarray.

Return the length of the longest possible equal subarray after deleting at most k elements from nums.

A subarray is a contiguous, possibly empty sequence of elements within an array.

## Constraints

- 1 <= nums.length <= 105
- 1 <= nums[i] <= nums.length
- 0 <= k <= nums.length

## Solution

- **Status:** Needed Hints

### Solution Notes

Came up with a valid solution on my own. Implementation was pretty bad. Should have been much simpler. Needed to look at discussion to see better way to go about it.


### Sorting

```go
package main
unc longestEqualSubarray(nums []int, k int) int {
    if len(nums) == 1 { return 1}
    numsWithIndex := make([][2]int, len(nums))
    for i, n := range nums { numsWithIndex[i] = [2]int{i, n} }

    // Group nums with index in ascending order via sort
    sort.Slice(numsWithIndex, func(i, j int) bool {
        if numsWithIndex[i][1] == numsWithIndex[j][1] {
            return numsWithIndex[i][0] < numsWithIndex[j][0]
        }
        return numsWithIndex[i][1] < numsWithIndex[j][1]
    })

    left, right := 0, 1
    longest := 1
    for right < len(numsWithIndex) { 
        dist := 0
        for right < len(numsWithIndex) && numsWithIndex[left][1] == numsWithIndex[right][1] {
            dist += numsWithIndex[right][0] - numsWithIndex[right-1][0]-1
            for dist > k {
                left++
                dist -= numsWithIndex[left][0] - numsWithIndex[left-1][0]-1             
            }         
            longest = max(longest, right-left+1)
            right++               
        }
        left = right
        right++
    }
    return longest
}

func max(x, y int) int { if x > y { return x }; return y }
```

### Hash Map

```go
package main
```
