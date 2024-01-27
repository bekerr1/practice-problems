# Queue Reconstruction by Height

## Metadata

- **Date Solved:** January 27, 2024 4:34 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/queue-reconstruction-by-height/description/
- **Algorithm:** binary index tree, sort
- **Type:** array

## Problem Statement

You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.

Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).

## Constraints

- 1 <= people.length <= 2000
- 0 <= hi <= 106
- 0 <= ki < people.length
- It is guaranteed that the queue can be reconstructed.

## Solution

- **Status:** Solved On Own


```go
package main

import "sort"

type BIT [1000002]int

func (b *BIT) add(key int) {
	key += 1
	for i := key; i > 0; i -= (i & (-i)) {
		b[i]++
	}
}

func (b *BIT) query(key int) int {
	key += 1
	res := 0
	for i := key; i < len(b); i += (i & (-i)) {
		res += b[i]
	}
	return res
}

func reconstructQueue(people [][]int) [][]int {

	sort.Slice(people, func(i, j int) bool {
		if people[i][1] == people[j][1] {
			return people[i][0] < people[j][0]
		}
		return people[i][1] < people[j][1]
	})
	k := [20001][]int{}
	for _, p := range people {
		k[p[1]] = append(k[p[1]], p[0])
	}

	bit := BIT{}
	ans := make([][]int, 0, len(people))
	for i := len(ans); i >= 0; i-- {
		for len(k[i]) > 0 && bit.query(k[i][0]) == i {
			ans = append(ans, []int{k[i][0], i})
			bit.add(k[i][0])
			k[i] = k[i][1:]
			i = len(ans)
		}
	}
	return ans
}
```
