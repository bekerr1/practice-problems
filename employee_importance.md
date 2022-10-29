# Employee Importance

## Metadata

- **Date Solved:** October 29, 2022 9:47 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/employee-importance/
- **Algorithm:** bfs, hashmap
- **Type:** array, scenario

## Problem Statement

You have a data structure of employee information, including the employee's unique ID, importance value, and direct subordinates' IDs.
You are given an array of employees employees where:
- employees[i].id is the ID of the ith employee.
- employees[i].importance is the importance value of the ith employee.
- employees[i].subordinates is a list of the IDs of the direct subordinates of the ith employee.
Given an integer id that represents an employee's ID, return the total importance value of this employee and all their direct and indirect subordinates.

## Constraints

- 1 <= employees.length <= 2000
- 1 <= employees[i].id <= 2000
- All employees[i].id are unique.
- -100 <= employees[i].importance <= 100
- One employee has at most one direct leader and may have several subordinates.
- The IDs in employees[i].subordinates are valid IDs.

## Solution

- **Status:** Solved On Own

### Solution Notes

Keeping track of the importance


```go
package main

func getImportance(employees []*Employee, id int) int {
	empMap := make(map[int]*Employee)
	for i, emp := range employees {
		empMap[emp.Id] = employees[i]
	}

	queue := []int{}
	queue = append(queue, id)
	importance := 0

	for len(queue) > 0 {
		count := len(queue)
		for count > 0 {
			curID := queue[0]
			queue = queue[1:]
			count--

			importance += empMap[curID].Importance
			queue = append(queue, empMap[curID].Subordinates...)
		}
	}
	return importance
}
```
