# Binary Tree Right Side View

## Metadata

- **Date Solved:** February 18, 2025 10:09 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/binary-tree-right-side-view/description/
- **Algorithm:** bfs, dfs
- **Type:** binary tree

## Problem Statement

Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

## Constraints


- The number of nodes in the tree is in the range [0, 100].
- -100 <= Node.val <= 100

## Solution

- **Status:** Solved On Own


```go
package main

func rightSideView(root *TreeNode) []int {
	if root == nil {
		return []int{}
	}
	queue := []*TreeNode{root}
	ans := []int{}
	for len(queue) > 0 {
		qLen := len(queue)
		for i := 0; i < qLen; i++ {
			cur := queue[0]
			queue = queue[1:]
			if cur.Right != nil {
				queue = append(queue, cur.Right)
			}
			if cur.Left != nil {
				queue = append(queue, cur.Left)
			}
			if i > 0 {
				continue
			}
			ans = append(ans, cur.Val)
		}
	}
	return ans
}
```
