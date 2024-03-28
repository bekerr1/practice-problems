# Reorder List

## Metadata

- **Date Solved:** March 28, 2024 12:53 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/reorder-list/description/
- **Algorithm:** brute force
- **Type:** linked list

## Problem Statement

You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln

Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

## Constraints


- The number of nodes in the list is in the range [1, 5 * 104].
- 1 <= Node.val <= 1000

## Solution

- **Status:** Looked At Discuss


```go
package main

func reorderList(head *ListNode) {
	if head.Next == nil {
		return
	}
	var tmp *ListNode
	prev, mid, end := head, head, head
	// find middle
	for end != nil && end.Next != nil {
		prev = mid
		mid, end = mid.Next, end.Next.Next
	}
	prev.Next = nil

	// reverse
	prev = nil
	for mid != nil {
		tmp = mid.Next
		mid.Next = prev
		prev = mid
		mid = tmp
	}

	// combined
	tmp = &ListNode{}
	t, rev := tmp, prev
	for i := 0; rev != nil || head != nil; i++ {
		if i%2 == 0 && head != nil {
			t.Next = head
			head, t = head.Next, t.Next
		}
		if i%2 != 0 && rev != nil {
			t.Next = rev
			rev, t = rev.Next, t.Next
		}
	}
	head = tmp.Next
}
```
