# Minimum Operations to Maximize Last Elements in Arrays

## Metadata

- **Date Solved:** November 19, 2023 11:03 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/minimum-operations-to-maximize-last-elements-in-arrays/description/
- **Algorithm:** math, two pass
- **Type:** array

## Problem Statement

You are given two 0-indexed integer arrays, nums1 and nums2, both having length n.
You are allowed to perform a series of operations(possibly none).

In an operation, you select an index i in the range [0, n - 1] and swap the values of nums1[i] and nums2[i].
Your task is to find the minimum number of operations required to satisfy the following conditions:

- nums1[n - 1] is equal to the maximum value among all elements of nums1, i.e., nums1[n - 1] = max(nums1[0], nums1[1], ..., nums1[n - 1]).
- nums2[n - 1] is equal to the maximum value among all elements of nums2, i.e., nums2[n - 1] = max(nums2[0], nums2[1], ..., nums2[n - 1]).

Return an integer denoting the minimum number of operations needed to meet both conditions, or -1 if it is impossible to satisfy both conditions.

## Constraints

- 1 <= n == nums1.length == nums2.length <= 1000
- 1 <= nums1[i] <= 109
- 1 <= nums2[i] <= 109

## Solution

- **Status:** Needed Hints

### Solution Notes

Started to consider DP after no looping solution was coming to mind. DP solution was a dud. Started to work with a two pass after looking at solution submission headers only. Finally came up with something but it required a lot of re-word/printing/submitting. Not good. Final solution though makes sense.

Going forward would have been better to nail down some conditions that would hold early and try to work with them. Considering generic min/max values compared to current value.


```go
package main

func minOperations(nums1 []int, nums2 []int) int {
	N := len(nums1)
	ans1 := 0
	minRow, maxRow := minMaxRows(N-1, nums1, nums2)
	for i := 0; i < N; i++ {
		// Check these general conditions only on first pass.
		// No need to do them on second.
		if max(minRow[i], maxRow[i]) > max(minRow[N-1], maxRow[N-1]) {
			return -1
		}
		if min(minRow[i], maxRow[i]) > min(minRow[N-1], maxRow[N-1]) {
			return -1
		}

		if minRow[i] > minRow[N-1] {
			ans1++
		}
	}

	// Account for swap
	ans2 := 1
	nums1[N-1], nums2[N-1] = nums2[N-1], nums1[N-1]
	minRow, maxRow = maxRow, minRow
	for i := N - 2; i >= 0; i-- {
		if minRow[i] > minRow[N-1] {
			ans2++
		}
	}
	return min(ans1, ans2)
}

func minMaxRows(n int, n1, n2 []int) ([]int, []int) {
	if n1[n] < n2[n] {
		return n1, n2
	}
	return n2, n1
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

### Lee Solution

I like this one. Seems like an optimized version of mine, without all the clutter.

```go
package main
int minOperations(vector<int>& A, vector<int>& B) {
        int dp1 = 0, dp2 = 0, n = A.size(), mi = min(A[n - 1], B[n - 1]), ma = max(A[n - 1], B[n - 1]);
        for (int i = 0; i < n; i++) {
            int a = A[i], b = B[i];
            if (max(a, b) > ma) return -1;
            if (min(a, b) > mi) return -1;
            if (a > A[n - 1] || b > B[n - 1]) dp1++;
            if (a > B[n - 1] || b > A[n - 1]) dp2++;
        }
        return min(dp1, dp2);
    }
```
