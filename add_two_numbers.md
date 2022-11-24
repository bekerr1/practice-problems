# Add Two Numbers

## Metadata

- **Date Solved:** November 23, 2022 9:57 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/add-two-numbers/
- **Algorithm:** brute force
- **Type:** linked list

## Problem Statement

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## Constraints

- The number of nodes in each linked list is in the range [1, 100].
- 0 <= Node.val <= 9
- It is guaranteed that the list represents a number that does not have leading zeros.

## Solution

- **Status:** Solved On Own

### Solution Notes

creating a dummy node and returning http://dummy.Next would have been a good approach from the beggining


```go
package main

func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
	node := &ListNode{}
	tmp := node
	carryOver := 0
	for l1 != nil || l2 != nil || carryOver == 1 {
		sum := 0
		if l1 != nil {
			sum += l1.Val
			l1 = l1.Next
		}
		if l2 != nil {
			sum += l2.Val
			l2 = l2.Next
		}
		sum += carryOver
		carryOver = sum / 10
		tmp.Next = &ListNode{
			Val: sum % 10,
		}
		tmp = tmp.Next
	}
	return node.Next
}
```
