# Binary Tree Paths

## Metadata

- **Date Solved:** August 14, 2022 11:27 AM
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/binary-tree-paths/
- **Algorithm:** dfs
- **Type:** tree

## Problem Statement

Given the root of a binary tree, return all root-to-leaf paths in any order.
A leaf is a node with no children.

## Problem


```go
package main

import "strconv"

/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func binaryTreePaths(root *TreeNode) []string {
	paths := []string{}
	pathsPtr := &paths
	binaryTreePathsUtil(root, "", pathsPtr)
	return *pathsPtr
}

func binaryTreePathsUtil(node *TreeNode, curPath string, paths *[]string) {
	if node == nil {
		return
	}
	if curPath == "" {
		curPath = strconv.Itoa(node.Val)
	} else {
		curPath = curPath + "->" + strconv.Itoa(node.Val)
	}

	if node.Left == nil && node.Right == nil {
		*paths = append(*paths, curPath)
		return
	}

	binaryTreePathsUtil(node.Left, curPath, paths)
	binaryTreePathsUtil(node.Right, curPath, paths)
}
```
