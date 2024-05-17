# Delete Leaves With a Given Value

## Metadata

- **Date Solved:** May 16, 2024 10:50 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/delete-leaves-with-a-given-value/description/?envType=daily-question&envId=2024-05-17
- **Algorithm:** dfs
- **Type:** binary tree, tree

## Problem Statement

Given a binary tree root and an integer target, delete all the leaf nodes with value target.

Note that once you delete a leaf node with value target, if its parent node becomes a leaf node and has the value target, it should also be deleted (you need to continue doing that until you cannot).

## Constraints


- The number of nodes in the tree is in the range [1, 3000].
- 1 <= Node.val, target <= 1000

## Solution

- **Status:** Solved On Own


```go
package main

func removeLeafNodes(root *TreeNode, target int) *TreeNode {
	if solve(root, target) {
		return nil
	}
	return root
}

func solve(node *TreeNode, target int) bool {
	if node == nil {
		return false
	}

	if solve(node.Left, target) {
		node.Left = nil
	}
	if solve(node.Right, target) {
		node.Right = nil
	}
	if node.Left == nil && node.Right == nil && node.Val == target {
		return true
	}
	return false
}
```

```go
package main

func removeLeafNodes(root *TreeNode, target int) *TreeNode {
	return solve(root, target)
}

func solve(node *TreeNode, target int) *TreeNode {
	if node == nil {
		return node
	}

	node.Left = solve(node.Left, target)
	node.Right = solve(node.Right, target)

	if node.Left == nil && node.Right == nil && node.Val == target {
		return nil
	}
	return node
}
```
