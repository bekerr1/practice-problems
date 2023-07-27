# Fair Distribution of Cookies

## Metadata

- **Date Solved:** July 26, 2023 8:06 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/fair-distribution-of-cookies/description/
- **Algorithm:** backtracking, bitmask, dynamic programming
- **Type:** array

## Problem Statement

You are given an integer array cookies, where cookies[i] denotes the number of cookies in the ith bag. You are also given an integer k that denotes the number of children to distribute all the bags of cookies to. All the cookies in the same bag must go to the same child and cannot be split up.

The unfairness of a distribution is defined as the maximum total cookies obtained by a single child in the distribution.

Return the minimum unfairness of all distributions.

## Constraints

- 2 <= cookies.length <= 8
- 1 <= cookies[i] <= 105
- 2 <= k <= cookies.length

## Solution

- **Status:** Looked At Discuss


### Backtracking

```go
package main

import "math"

var kSets []int

func distributeCookies(cookies []int, k int) int {
	kSets = make([]int, k)
	return distributeCookiesUtil(cookies, 0, math.MaxInt)
}

func distributeCookiesUtil(cookies []int, start int, ans int) int {

	if start == len(cookies) {
		maxK := math.MinInt
		for i := range kSets {
			maxK = max(kSets[i], maxK)
		}
		return maxK
	}

	for i := 0; i < len(kSets); i++ {
		kSets[i] += cookies[start]
		ans = min(ans, distributeCookiesUtil(cookies, start+1, ans))
		kSets[i] -= cookies[start]
	}
	return ans
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
