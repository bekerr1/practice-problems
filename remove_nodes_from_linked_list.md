# Remove Nodes From Linked List

## Metadata

- **Date Solved:** March 5, 2023 6:55 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/remove-nodes-from-linked-list/description/
- **Algorithm:** monotonic stack, recursion
- **Type:** linked list

## Problem Statement

You are given the head of a linked list.
Remove every node which has a node with a strictly greater value anywhere to the right side of it.
Return the head of the modified linked list.

## Constraints

- The number of the nodes in the given list is in the range [1, 105].
- 1 <= Node.val <= 105

## Solution

- **Status:** Solved On Own

### Solution Notes

classic monotonic stack problem


```go
package main

func removeNodes(head *ListNode) *ListNode {
	tmp := &ListNode{}
	monoStack := []*ListNode{tmp}
	topIdx := 0
	for node := head; node != nil; node = node.Next {
		for topIdx > 0 && monoStack[topIdx].Val < node.Val {
			monoStack = monoStack[0:topIdx]
			topIdx--
		}
		monoStack[topIdx].Next = node
		monoStack = append(monoStack, node)
		topIdx++
	}
	return tmp.Next
}
```
