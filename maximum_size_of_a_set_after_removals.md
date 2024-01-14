# Maximum Size of a Set After Removals

## Metadata

- **Date Solved:** January 14, 2024 4:48 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-size-of-a-set-after-removals/description/
- **Algorithm:** hashmap
- **Type:** array, math

## Problem Statement

You are given two 0-indexed integer arrays nums1 and nums2 of even length n.

You must remove n / 2 elements from nums1 and n / 2 elements from nums2. After the removals, you insert the remaining elements of nums1 and nums2 into a set s.

Return the maximum possible size of the set s.

## Constraints

- n == nums1.length == nums2.length
- 1 <= n <= 2 * 104
- n is even.
- 1 <= nums1[i], nums2[i] <= 109

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I tried to solve this problem with a heap initially by trying to parse out repeated values to add variability between the final set. This didn't work. It was really complicated logic to understand when to add the entry back + take an entry for a certain set. 

With this I was trying to find the numbers not to take. The discussion talks about finding the numbers TO take. In this way, we would do the opposite of what I was looking to do. This means, finding entries that are unique to a certain set, then for the rest take  repeated numbers.


```go
package main

func maximumSetSize(nums1 []int, nums2 []int) int {
	N := len(nums1)
	s1, s2, s := make(map[int]bool), make(map[int]bool), make(map[int]bool)
	for _, n := range nums1 {
		s1[n] = true
		s[n] = true
	}
	for _, n := range nums2 {
		s2[n] = true
		s[n] = true
	}

	common := len(s1) + len(s2) - len(s)
	unique1 := min(N/2, len(s1)-common)
	unique2 := min(N/2, len(s2)-common)
	return min(N, unique1+unique2+common)

}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

### Explanation

nums1 = {1, 1, 1, 2, 2, 3}

nums2 = {3, 3, 4, 4, 5, 5}

| Set1 | Set2 |
| --- | --- |
| 1 | 3 |
| 2 | 4 |
| 3 | 5 |

| Union Set1 | Set2 |
| --- |
| 1 |
| 2 |
| 3 |
| 4 |
| 5 |

If we create sets from nums1, nums2, and a set from all entries between both, we can obtain the common nums by adding len(set1) + len(set2) - len(union). From example above this would be 3 + 3 - 5 = 1 (would account for the '3' shared between num1 and num2). From there we would use the common num to find the count of uniques for each.

