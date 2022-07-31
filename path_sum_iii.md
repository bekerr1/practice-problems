# Path Sum III

## Metadata

- **Date Solved:** July 31, 2022 4:34 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/path-sum-iii/
- **Algorithm:** dfs, prefixsum
- **Type:** tree

## Problem Statement

Given the root of a binary tree and an integer targetSum, return the number of paths where the sum of the values along the path equals targetSum.
The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

## Problem


### Brute Force

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
func pathSum(root *TreeNode, targetSum int) int {
	sums := 0
	s := &sums
	pathSumUtil(root, targetSum, s)
	return *s
}

func pathSumUtil(node *TreeNode, targetSum int, equalPaths *int) {

	if node == nil {
		return
	}

	pathSumForNode(node, targetSum, equalPaths)

	pathSumUtil(node.Left, targetSum, equalPaths)
	pathSumUtil(node.Right, targetSum, equalPaths)
}

func pathSumForNode(node *TreeNode, targetSum int, equalPaths *int) {
	if node == nil {
		return
	}
	targetSum -= node.Val
	if targetSum == 0 {
		*equalPaths += 1
	}
	pathSumForNode(node.Left, targetSum, equalPaths)
	pathSumForNode(node.Right, targetSum, equalPaths)
}
```

### Optimized - PrefixSum

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
func pathSum(root *TreeNode, targetSum int) int {
	prefixSums := make(map[int]int)
	prefixSums[0] = 1
	p := 0
	paths := &p
	pathSumUtil(root, targetSum, 0, prefixSums, paths)
	return *paths
}

func pathSumUtil(
	node *TreeNode,
	targetSum, currentSum int,
	prefixSums map[int]int,
	paths *int,
) {
	if node == nil {
		return
	}

	currentSum += node.Val
	if cnt, exists := prefixSums[currentSum-targetSum]; exists {
		*paths += cnt
	}

	prefixSums[currentSum] += 1
	pathSumUtil(node.Left, targetSum, currentSum, prefixSums, paths)
	pathSumUtil(node.Right, targetSum, currentSum, prefixSums, paths)
	prefixSums[currentSum] -= 1
}
```
