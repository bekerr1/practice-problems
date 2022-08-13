# Sum Root to Leaf Numbers

## Metadata

- **Date Solved:** August 12, 2022 11:43 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/sum-root-to-leaf-numbers/
- **Algorithm:** dfs
- **Type:** tree

## Problem Statement

You are given the root of a binary tree containing digits from 0 to 9 only.
Each root-to-leaf path in the tree represents a number.
- For example, the root-to-leaf path 1 -> 2 -> 3 represents the number 123.
Return the total sum of all root-to-leaf numbers. Test cases are generated so that the answer will fit in a 32-bit integer.
A leaf node is a node with no children.

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
func sumNumbers(root *TreeNode) int {
	if root == nil {
		return 0
	}
	return sumNumbersUtil(root, 0)
}

func sumNumbersUtil(node *TreeNode, curNum int) int {
	var sum int
	num := 10*curNum + node.Val
	if node.Left == nil && node.Right == nil {
		return num
	}

	if node.Left != nil {
		sum += sumNumbersUtil(node.Left, num)
	}
	if node.Right != nil {
		sum += sumNumbersUtil(node.Right, num)
	}
	return sum
}
```

## Solution Statement


At first I tried to solve this problem bottom up which was not right. The problem demands that each path from the root towards the leaf keep track of the current sum. In the bottom up approach, I was trying to solve the problem from using the depth and calculating the sum from the 1, 10s, 100s, etc place. In this way we cant keep track of what number to pass up from leaf to root.

Instead, working towards the leaf and keeping track of the current sum while multiplying by 10 each round will increase the current number by the correct ‘10s place interval’.
