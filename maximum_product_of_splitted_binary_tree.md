# Maximum Product of Splitted Binary Tree

## Metadata

- **Date Solved:** December 22, 2022 9:13 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-product-of-splitted-binary-tree/description/
- **Algorithm:** dfs
- **Type:** binary tree

## Problem Statement

Given the root of a binary tree, split the binary tree into two subtrees by removing one edge such that the product of the sums of the subtrees is maximized.
Return the maximum product of the sums of the two subtrees. Since the answer may be too large, return it modulo 109 + 7.
Note that you need to maximize the answer before taking the mod and not after taking it.

## Constraints

- The number of nodes in the tree is in the range [2, 5 * 104].
- 1 <= Node.val <= 104

## Solution

- **Status:** Looked At Discuss

### Solution Notes

total = sum1 + sum2
min(abs(sum1 - sum2))
sum1 = sum2 = total/2
sum1 == total/2

create math equations to come up with a solution for the max sum


```go
package main

var totalSum, maxSum int
var mod = 1000000000 + 7

func maxProduct(root *TreeNode) int {
	totalSum, maxSum = 0, 0
	totalSum = maxProductUtil(root)
	maxProductUtil(root)
	return maxSum % mod
}

func maxProductUtil(node *TreeNode) int {
	if node == nil {
		return 0
	}
	sum := maxProductUtil(node.Left) + maxProductUtil(node.Right) + node.Val
	maxSum = max(maxSum, (totalSum-sum)*sum)
	return sum
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
