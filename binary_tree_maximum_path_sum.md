# Binary Tree Maximum Path Sum

## Metadata

- **Date Solved:** July 13, 2022 7:28 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/binary-tree-maximum-path-sum/
- **Algorithm:** dfs
- **Type:** tree

## Problem Statement

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.
The path sum of a path is the sum of the node's values in the path.
Given the root of a binary tree, return the maximum path sum of any non-empty path.

## Problem


```go
package main

/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
import "math"

func maxPathSum(root *TreeNode) int {
	m := math.MinInt
	maxRef := &m
	maxPathSumUtil(root, maxRef)
	return *maxRef
}

func maxPathSumUtil(node *TreeNode, maxRef *int) int {
	if node == nil {
		return 0
	}
	leftMax := max(0, maxPathSumUtil(node.Left, maxRef))
	rightMax := max(0, maxPathSumUtil(node.Right, maxRef))
	*maxRef = max(*maxRef, leftMax+rightMax+node.Val)
	return max(leftMax, rightMax) + node.Val
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
