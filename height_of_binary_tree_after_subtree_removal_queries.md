# Height of Binary Tree After Subtree Removal Queries

## Metadata

- **Date Solved:** November 1, 2024 9:15 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/height-of-binary-tree-after-subtree-removal-queries/description/?envType=daily-question&envId=2024-10-26
- **Algorithm:** dfs, hashmap, math
- **Type:** binary tree, tree

## Problem Statement

You are given the root of a binary tree with n nodes. Each node is assigned a unique value from 1 to n. You are also given an array queries of size m.
You have to perform m independent queries on the tree where in the ith query you do the following:

- Remove the subtree rooted at the node with the value queries[i] from the tree. It is guaranteed that queries[i] will not be equal to the value of the root.
Return an array answer of size m where answer[i] is the height of the tree after performing the ith query.

Note:
- The queries are independent, so the tree returns to its initial state after each query.
- The height of a tree is the number of edges in the longest simple path from the root to some node in the tree.

## Constraints


- The number of nodes in the tree is n.
- 2 <= n <= 105
- 1 <= Node.val <= n
- All the values in the tree are unique.
- m == queries.length
- 1 <= m <= min(n, 104)
- 1 <= queries[i] <= n
- queries[i] != root.val

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Initially I tried hard to come up with a solution that included calculating the total height at any given node or propogating a query back up the tree. Really calculating the height and maintaining node depth was the key. The solution i found most sensical was solving the max height of any given cousin. This involved keeping track of the first and second heighest height at a given depth.


```go
package main

type n struct {
	node   int
	height int
}

var nodeDepth [100001]int
var nodeDepthMetrics [100001][2]n

func treeQueries(root *TreeNode, queries []int) []int {
	nodeDepth = [100001]int{}
	nodeDepthMetrics = [100001][2]n{{{root.Val, 0}, {root.Val, 0}}}

	getMetrics(root, 0)

	ans := make([]int, len(queries))
	for i, q := range queries {
		noDepth := nodeDepth[q]
		ans[i] = noDepth + nodeDepthMetrics[noDepth][0].height - 1
		if nodeDepthMetrics[noDepth][0].node == q {
			ans[i] = noDepth + nodeDepthMetrics[noDepth][1].height - 1
		}
	}
	return ans
}

func getMetrics(node *TreeNode, depth int) int {
	if node == nil {
		return 0
	}

	nodeDepth[node.Val] = depth
	l := getMetrics(node.Left, depth+1)
	r := getMetrics(node.Right, depth+1)

	height := 1 + max(l, r)

	dm0 := nodeDepthMetrics[depth][0]
	dm1 := nodeDepthMetrics[depth][1]

	if height > dm0.height {
		dm1 = dm0
		dm0 = n{node.Val, height}
	} else if height > dm1.height {
		dm1 = n{node.Val, height}
	}

	nodeDepthMetrics[depth][0] = dm0
	nodeDepthMetrics[depth][1] = dm1

	return height
}
```
