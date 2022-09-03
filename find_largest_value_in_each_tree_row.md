# Find Largest Value in Each Tree Row

## Metadata

- **Date Solved:** September 3, 2022 12:22 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-largest-value-in-each-tree-row/
- **Algorithm:** bfs, dfs
- **Type:** binary tree

## Problem Statement

Given the root
 of a binary tree, return an array of the largest value in each row
 of the tree (0-indexed)

## Constraints

- The number of nodes in the tree will be in the range [0, 104].
- -231 <= Node.val <= 231 - 1

## Solution


### DFS

```go
package main
```

### BFS

```go
package main

import "math"

/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func largestValues(root *TreeNode) []int {
	if root == nil {
		return []int{}
	}
	queue := []*TreeNode{}
	queue = append(queue, root)
	maxVals := []int{}
	for len(queue) > 0 {
		lenAtDepth := len(queue)
		maxAtDepth := math.MinInt
		for lenAtDepth > 0 {
			curNode := queue[0]
			queue = queue[1:len(queue)]
			maxAtDepth = max(maxAtDepth, curNode.Val)
			if curNode.Left != nil {
				queue = append(queue, curNode.Left)
			}
			if curNode.Right != nil {
				queue = append(queue, curNode.Right)
			}
			lenAtDepth--
		}
		maxVals = append(maxVals, maxAtDepth)
	}
	return maxVals
}

func max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```
