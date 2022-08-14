# Smallest String Starting From Leaf

## Metadata

- **Date Solved:** August 14, 2022 10:59 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/smallest-string-starting-from-leaf/
- **Algorithm:** dfs
- **Type:** tree

## Problem Statement

You are given the root of a binary tree where each node has a value in the range [0, 25] representing the letters 'a' to 'z'.
Return the lexicographically smallest string that starts at a leaf of this tree and ends at the root.
As a reminder, any shorter prefix of a string is lexicographically smaller.
- For example, "ab" is lexicographically smaller than "aba".
A leaf of a node is a node that has no children.

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
func smallestFromLeaf(root *TreeNode) string {
	empty := ""
	minString := &empty
	smallestFromLeafUtil(root, "", minString)
	return *minString
}

func smallestFromLeafUtil(node *TreeNode, strSoFar string, curMin *string) {
	if node == nil {
		return
	}
	strSoFar = string(node.Val+'a') + strSoFar
	if node.Left == nil && node.Right == nil {
		if *curMin == "" {
			*curMin = strSoFar
		} else {
			*curMin = min(*curMin, strSoFar)
		}
		return
	}
	smallestFromLeafUtil(node.Left, strSoFar, curMin)
	smallestFromLeafUtil(node.Right, strSoFar, curMin)
}

func min(strX, strY string) string {
	if strX < strY {
		return strX
	}
	return strY
}
```

## Solution Statement


Similar to ‘Sum Root to Leaf Numbers’, I tried to start the problem with a bottom up approach and failed. There was a case where the left and right nodes returned ‘ba’ and ‘b’ respectively. In this case, ‘b’ was smaller than ‘ba’ but in the calling stack ‘be’ would be formed when ‘bae’ would be considered the actual min. Because the order of the string is from leaf to root, analyzing the string from bottom up will not do. The parent node has effect on the min-ness of the string.
