# Pseudo-Palindromic Paths in a Binary Tree

## Metadata

- **Date Solved:** January 24, 2024 7:21 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/pseudo-palindromic-paths-in-a-binary-tree/description/?envType=daily-question&envId=2024-01-24
- **Algorithm:** dfs
- **Type:** binary tree

## Problem Statement

Given a binary tree where node values are digits from 1 to 9. A path in the binary tree is said to be pseudo-palindromic if at least one permutation of the node values in the path is a palindrome.

Return the number of pseudo-palindromic paths going from the root node to leaf nodes.

## Constraints

- The number of nodes in the tree is in the range [1, 105].
- 1 <= Node.val <= 9

## Solution

- **Status:** Solved On Own

### Solution Notes

I was able to get the intuition behind this pretty quick. Because the node values are only 1-9, we can detect a palindromic permutation in constant time (O(9)).

One thing that tripped me up a bit was the constraint. I was not checking for palindromic permutation at the leaf node specifically (left && right == nil). This caused some test cases to trip. For future, it's best to account for this from the start.

Anytime a problem asks for specific leaf node condition, need to remember to likely check for it explicitly.  


```go
package main

func pseudoPalindromicPaths(root *TreeNode) int {
	counts := [10]int{}
	return solve(root, &counts)
}

func solve(node *TreeNode, counts [10]int) int {
	if node == nil {
		return 0
	}

	ans := 0
	counts[node.Val]++

	// We are at leaf
	if node.Left == nil && node.Right == nil {
		if isPalindromic(counts) {
			ans++
		}
	} else {
		ans += solve(node.Left, counts)
		ans += solve(node.Right, counts)
	}
	counts[node.Val]--
	return ans
}

func isPalindromic(counts [10]int) bool {
	odds := 0
	for _, c := range counts {
		switch c % 2 {
		case 1:
			odds++
		}
	}
	return odds == 0 || odds == 1
}
```
