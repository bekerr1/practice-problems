# Minimum Flips in Binary Tree to Get Result

## Metadata

- **Date Solved:** July 3, 2024 11:41 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/minimum-flips-in-binary-tree-to-get-result/description/?envType=study-plan-v2&envId=google-spring-23-high-frequency
- **Algorithm:** dfs, dynamic programming
- **Type:** binary tree, bits, tree

## Problem Statement

You are given the root of a binary tree with the following properties:
- Leaf nodes have either the value 0 or 1, representing false and truerespectively.

- Non-leaf nodes have either the value 2, 3, 4, or 5, representing the boolean operations OR, AND, XOR, and NOT, respectively.
You are also given a boolean result, which is the desired result of the evaluationof the root node.

The evaluation of a node is as follows:

- If the node is a leaf node, the evaluation is the value of the node, i.e. true or false.
- Otherwise, evaluate the node's children and apply the boolean operation of its value with the children's evaluations.

In one operation, you can flip a leaf node, which causes a false node to become true, and a true node to become false.

Return the minimum number of operations that need to be performed such that the evaluation of root yields result. It can be shown that there is always a way to achieve result.

A leaf node is a node that has zero children.

Note: NOT nodes have either a left child or a right child, but other non-leaf nodes have both a left child and a right child.

## Constraints


- The number of nodes in the tree is in the range [1, 105].
- 0 <= Node.val <= 5
- OR, AND, and XOR nodes have 2 children.
- NOT nodes have 1 child.
- Leaf nodes have a value of 0 or 1.
- Non-leaf nodes have a value of 2, 3, 4, or 5.

## Solution

- **Status:** Needed Hints


```go
package main

import "math"

var andTable = [2][][2]int{{{1, 0}, {0, 1}, {0, 0}}, {{1, 1}}}
var orTable = [2][][2]int{{{0, 0}}, {{0, 1}, {1, 0}, {1, 1}}}
var xorTable = [2][][2]int{{{1, 1}, {0, 0}}, {{1, 0}, {0, 1}}}
var opsMap = map[int][2][][2]int{
	2: orTable,
	3: andTable,
	4: xorTable,
}

type Pair struct {
	tn  *TreeNode
	idx int
}

var memo map[Pair]int

func minimumFlips(root *TreeNode, result bool) int {
	memo = make(map[Pair]int)
	return solve(root, bitForBool(result))
}

func solve(root *TreeNode, result int) int {
	if root.Left == nil && root.Right == nil {
		if root.Val == result {
			return 0
		}
		return 1
	}

	if ans, ok := memo[Pair{root, result}]; ok {
		return ans
	}
	ans := math.MaxInt
	if root.Val == 5 {
		if root.Left != nil {
			return solve(root.Left, flip(result))
		} else {
			return solve(root.Right, flip(result))
		}
	} else {
		for _, d := range opsMap[root.Val][result] {
			ansLeft := solve(root.Left, d[0])
			ansRight := solve(root.Right, d[1])
			ans = min(ans, ansLeft+ansRight)
			if ans == 0 {
				break
			}
		}
	}
	memo[Pair{root, result}] = ans
	return ans
}

func bitForBool(b bool) int {
	if b {
		return 1
	}
	return 0
}
func flip(b int) int {
	if b == 0 {
		return 1
	}
	return 0
}
```
