# Time Needed to Inform All Employees

## Metadata

- **Date Solved:** July 23, 2024 9:05 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/time-needed-to-inform-all-employees/
- **Algorithm:** bfs, dfs
- **Type:** binary tree, tree

## Problem Statement

A company has n employees with a unique ID for each employee from 0 to n - 1. The head of the company is the one with headID.

Each employee has one direct manager given in the manager array where manager[i] is the direct manager of the i-th employee, manager[headID] = -1. Also, it is guaranteed that the subordination relationships have a tree structure.

The head of the company wants to inform all the company employees of an urgent piece of news. He will inform his direct subordinates, and they will inform their subordinates, and so on until all employees know about the urgent news.

The i-th employee needs informTime[i] minutes to inform all of his direct subordinates (i.e., After informTime[i] minutes, all his direct subordinates can start spreading the news).

Return the number of minutes needed to inform all the employees about the urgent news.

## Constraints


- 1 <= n <= 105
- 0 <= headID < n
- manager.length == n
- 0 <= manager[i] < n
- manager[headID] == -1
- informTime.length == n
- 0 <= informTime[i] <= 1000
- informTime[i] == 0 if employee i has no subordinates.
- It is guaranteed that all the employees can be informed.

## Solution

- **Status:** Needed Hints

### Solution Notes

Time is global to all employees. If at one level of the tree one employee takes much longer, the other will go ahead and inform and continue. I simply did the sum of informTime when really the max to reach any leaf was the answer as it takes into account faster paths not increasing the total time


```go
package main

func numOfMinutes(n int, headID int, manager []int, informTime []int) int {
	adjList := make([][]int, n)
	for i := range manager {
		if i != headID && informTime[i] > 0 {
			adjList[manager[i]] = append(adjList[manager[i]], i)
		}
	}

	q := [][2]int{{headID, informTime[headID]}}
	ans := 0
	for len(q) > 0 {
		u := q[0]
		q = q[1:]
		for _, v := range adjList[u[0]] {
			q = append(q, [2]int{v, u[1] + informTime[v]})
		}
		ans = max(ans, u[1])
	}
	return ans
}
```
