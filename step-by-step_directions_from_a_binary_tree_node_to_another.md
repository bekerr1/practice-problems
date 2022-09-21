# Step-By-Step Directions From a Binary Tree Node to Another

## Metadata

- **Date Solved:** September 21, 2022 12:29 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/
- **Algorithm:** dfs
- **Type:** binary tree

## Problem Statement

You are given the root of a binary tree with n nodes. Each node is uniquely assigned a value from 1 to n. You are also given an integer startValue representing the value of the start node s, and a different integer destValue representing the value of the destination node t.
Find the shortest path starting from node s and ending at node t. Generate step-by-step directions of such path as a string consisting of only the uppercase letters 'L', 'R', and 'U'. Each letter indicates a specific direction:
- 'L' means to go from a node to its left child node.
- 'R' means to go from a node to its right child node.
- 'U' means to go from a node to its parent node.
Return the step-by-step directions of the shortest path from node s to node t.

## Constraints

- The number of nodes in the tree is n.
- 2 <= n <= 105
- 1 <= Node.val <= n
- All the values in the tree are unique.
- 1 <= startValue, destValue <= n
- startValue != destValue

## Solution

- **Status:** Needed Hints

### Solution Notes

Building the path string for both sides individually and then trimming for common substring prefix was key here. Also backtracking on the current path was helpful for memory consumption.


### Initial Approach (TLE)

```go
package main

func getDirections(root *TreeNode, startValue int, destValue int) string {
	path := []byte{}
	start := []byte{}
	end := []byte{}
	getDirectionsUtil(root, startValue, destValue, &path, &start, &end)
	return string(path)
}

const (
	NodeNone  = 0
	NodeStart = 1
	NodeEnd   = 2
)

func getDirectionsUtil(node *TreeNode, startV, endV int, path, startPath, endPath *[]byte) int {
	var currentNodeKey int
	var returnNodeKey int
	if node.Val == startV {
		currentNodeKey = NodeStart
	} else if node.Val == endV {
		currentNodeKey = NodeEnd
	}
	returnNodeKey = currentNodeKey

	var leftNodeKey int
	var rightNodeKey int
	if node.Left != nil {
		leftNodeKey = getDirectionsUtil(node.Left, startV, endV, path, startPath, endPath)
	}
	if node.Right != nil {
		rightNodeKey = getDirectionsUtil(node.Right, startV, endV, path, startPath, endPath)
	}

	if leftNodeKey == NodeStart || rightNodeKey == NodeStart {
		*startPath = append(*startPath, 'U')
		returnNodeKey = leftNodeKey + rightNodeKey
	}
	if leftNodeKey == NodeEnd || rightNodeKey == NodeEnd {
		if leftNodeKey == NodeEnd {
			*endPath = append([]byte{'L'}, *endPath...)
		} else {
			*endPath = append([]byte{'R'}, *endPath...)
		}
		returnNodeKey = leftNodeKey + rightNodeKey
	}
	// LCA, path should be completed
	if currentNodeKey != NodeNone && (leftNodeKey != NodeNone || rightNodeKey != NodeNone) ||
		leftNodeKey != NodeNone && rightNodeKey != NodeNone {
		*path = append(*startPath, *endPath...)
		return NodeNone
	}
	return returnNodeKey
}
```

### Solution with Hints

```go
package main

var startPathPtr, endPathPtr *[]byte

func getDirections(root *TreeNode, startValue int, destValue int) string {
	startPathPtr, endPathPtr = nil, nil
	curPath := []byte{}
	getDirectionsUtil(root, startValue, destValue, curPath)

	startPath, endPath := *startPathPtr, *endPathPtr
	var i int
	for i = 0; i < len(startPath) && i < len(endPath); i++ {
		if startPath[i] != endPath[i] {
			break
		}
	}
	startPath = startPath[i:]
	endPath = endPath[i:]

	for i := range startPath {
		startPath[i] = 'U'
	}
	return string(startPath) + string(endPath)
}

func getDirectionsUtil(node *TreeNode, startV, endV int, curPath []byte) {
	if node == nil {
		return
	}

	if node.Val == startV {
		startPathPtr = &[]byte{}
		*startPathPtr = append(*startPathPtr, curPath...)
	} else if node.Val == endV {
		endPathPtr = &[]byte{}
		*endPathPtr = append(*endPathPtr, curPath...)
	}

	if startPathPtr != nil && endPathPtr != nil {
		return
	}

	curPath = append(curPath, 'L')
	getDirectionsUtil(node.Left, startV, endV, curPath)
	curPath = curPath[0 : len(curPath)-1]

	curPath = append(curPath, 'R')
	getDirectionsUtil(node.Right, startV, endV, curPath)
	curPath = curPath[0 : len(curPath)-1]
}
```
