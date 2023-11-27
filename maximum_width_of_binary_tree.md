# Maximum Width of Binary Tree

## Metadata

- **Date Solved:** November 26, 2023 11:36 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-width-of-binary-tree/description/
- **Algorithm:** bfs
- **Type:** binary tree

## Problem Statement

Given the root of a binary tree, return the maximum width of the given tree.
The maximum width of a tree is the maximum width among all levels.

The width of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes that would be present in a complete binary tree extending down to that level are also counted into the length calculation.

It is guaranteed that the answer will in the range of a 32-bit signed integer.

## Constraints

- The number of nodes in the tree is in the range [1, 3000].
- -100 <= Node.val <= 100

## Solution

- **Status:** Looked At Discuss


```go
package main

func widthOfBinaryTree(root *TreeNode) int {
	root.Val = 0
	queue := []*TreeNode{root}
	maxWidth := 0
	for len(queue) > 0 {
		qlen := len(queue)
		front, back := queue[0], queue[qlen-1]
		maxWidth = max(maxWidth, back.Val-front.Val+1)
		for i := 0; i < qlen; i++ {
			cur := queue[0]
			queue = queue[1:]
			if cur.Left != nil {
				cur.Left.Val = cur.Val*2 + 1
				queue = append(queue, cur.Left)
			}
			if cur.Right != nil {
				cur.Right.Val = cur.Val*2 + 2
				queue = append(queue, cur.Right)
			}
		}
	}
	return maxWidth
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
