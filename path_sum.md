# Path Sum

## Metadata

- **Date Solved:** July 17, 2022 10:19 PM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/path-sum/
- **Algorithm:** dfs
- **Type:** tree

## Problem Statement

Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.
A leaf is a node with no children.

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
func hasPathSum(root *TreeNode, targetSum int) bool {
	if root == nil {
		return false
	}
	return hasPathSumUtil(root, targetSum, 0)
}

func hasPathSumUtil(node *TreeNode, targetSum, currentSum int) bool {
	if node == nil {
		return false
	}
	leftHas := hasPathSumUtil(node.Left, targetSum, currentSum+node.Val)
	rightHas := hasPathSumUtil(node.Right, targetSum, currentSum+node.Val)
	return leftHas || rightHas || (node.Left == nil &&
		node.Right == nil &&
		currentSum+node.Val == targetSum)
}
```
