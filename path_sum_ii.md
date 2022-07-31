# Path Sum II

## Metadata

- **Date Solved:** July 31, 2022 12:16 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/path-sum-ii/
- **Algorithm:** dfs
- **Type:** tree

## Problem Statement

Given the root of a binary tree and an integer targetSum, return all root-to-leaf paths where the sum of the node values in the path equals targetSum. Each path should be returned as a list of the node values, not node references.
A root-to-leaf path is a path starting from the root and ending at any leaf node. A leaf is a node with no children.

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

var sums [][]int

func pathSum(root *TreeNode, targetSum int) [][]int {
	sums = [][]int{}
	pathSumUtil(root, targetSum, []int{})
	return sums
}

func pathSumUtil(root *TreeNode, targetSum int, curPath []int) {
	if root == nil {
		return
	}
	curPath = append(curPath, root.Val)
	pathSumUtil(root.Left, targetSum-root.Val, curPath)
	pathSumUtil(root.Right, targetSum-root.Val, curPath)
	if root.Left == nil && root.Right == nil && targetSum-root.Val == 0 {
		pathCopy := []int{}
		pathCopy = append(pathCopy, curPath...)
		sums = append(sums, pathCopy)
	}
	return
}
```
