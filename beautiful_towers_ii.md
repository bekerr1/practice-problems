# Beautiful Towers II

## Metadata

- **Date Solved:** October 1, 2023 7:39 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/beautiful-towers-ii/description/
- **Algorithm:** monotonic stack
- **Type:** array

## Problem Statement

You are given a 0-indexed array maxHeights of n integers.
You are tasked with building n towers in the coordinate line. The ith tower is built at coordinate i and has a height of heights[i].

A configuration of towers is beautiful if the following conditions hold:

1. 1 <= heights[i] <= maxHeights[i]
2. heights is a mountain array.

Array heights is a mountain if there exists an index i such that

- For all 0 < j <= i, heights[j - 1] <= heights[j]
- For all i <= k < n - 1, heights[k + 1] <= heights[k]

Return the maximum possible sum of heights of a beautiful configuration of towers.

## Constraints

- 1 <= n == maxHeights <= 105
- 1 <= maxHeights[i] <= 109

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I was able to at least come up with the correct process for finding a solution. The trouble i had was actually coding it up. I tried to keep track of the sums in a way that didn't account for certain scenarios.


```go
package main
unc maximumSumOfHeights(maxHeights []int) int64 {
    heights := make([]int64, len(maxHeights))
    monostack := []int{-1}
    var curSum, res int64 = 0, 0
    for i := 0; i < len(maxHeights); i++ {
        for len(monostack) > 1 && maxHeights[monostack[len(monostack)-1]] > maxHeights[i] {
            j := monostack[len(monostack)-1]
            monostack = monostack[0:len(monostack)-1]
            curSum -= int64(1 * (j - monostack[len(monostack)-1])) * int64(maxHeights[j])
        }
        curSum += int64(1 * (i - monostack[len(monostack)-1])) * int64(maxHeights[i])
        monostack = append(monostack, i)
        heights[i] = curSum
    }
    
    monostack = []int{len(maxHeights)}
    curSum = 0    
    for i := len(maxHeights)-1; i >= 0; i-- {
        for len(monostack) > 1 && maxHeights[monostack[len(monostack)-1]] > maxHeights[i] {
            j := monostack[len(monostack)-1]
            monostack = monostack[0:len(monostack)-1]
            curSum -= int64(1 * -(j - monostack[len(monostack)-1])) * int64(maxHeights[j])
        }
        curSum += int64(1 * -(i - monostack[len(monostack)-1])) * int64(maxHeights[i])
        monostack = append(monostack, i)
        res = max(res, heights[i] + curSum - int64(maxHeights[i]))
    }
    return res
}

func max(x, y int64) int64 { if x > y { return x }; return y }
```
