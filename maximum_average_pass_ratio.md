# Maximum Average Pass Ratio

## Metadata

- **Date Solved:** September 11, 2022 12:51 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/maximum-average-pass-ratio/
- **Algorithm:** brute force, heap
- **Type:** array, pairs

## Problem Statement

There is a school that has classes of students and each class will be having a final exam. You are given a 2D integer array classes, where classes[i] = [passi, totali]. You know beforehand that in the ith class, there are totali total students, but only passi number of students will pass the exam.
You are also given an integer extraStudents. There are another extraStudents brilliant students that are guaranteed to pass the exam of any class they are assigned to. You want to assign each of the extraStudents students to a class in a way that maximizes the average pass ratio across all the classes.
The pass ratio of a class is equal to the number of students of the class that will pass the exam divided by the total number of students of the class. The average pass ratio is the sum of pass ratios of all the classes divided by the number of the classes.
Return the maximum possible average pass ratio after assigning the extraStudents students. Answers within 10-5 of the actual answer will be accepted.

## Constraints

- 1 <= classes.length <= 105
- classes[i].length == 2
- 1 <= passi <= totali <= 105
- 1 <= extraStudents <= 105

## Solution

- **Status:** Looked At Discuss

### Solution Notes

Don’t make assumptions about sorting unless you are sure they hold true


### Brute Force (TLE)

```go
package main

func maxAverageRatio(classes [][]int, extraStudents int) float64 {
	for extraStudents > 0 {
		maxChangeClassIdx := 0
		var maxChange float64
		for i, class := range classes {
			if maxChange < passRatioDifference(class) {
				maxChangeClassIdx = i
				maxChange = passRatioDifference(class)
			}
		}
		classes[maxChangeClassIdx][0]++
		classes[maxChangeClassIdx][1]++
		extraStudents--
	}
	var ret float64
	for _, class := range classes {
		ret += float64(class[0]) / float64(class[1])
	}
	return ret / float64(len(classes))
}

func passRatioDifference(class []int) float64 {
	return (float64((class[0] + 1)) / float64((class[1] + 1))) - (float64((class[0])) / float64((class[1])))
}
```

### Heap

```go
package main

import "container/heap"

func maxAverageRatio(classes [][]int, extraStudents int) float64 {
	passRatioHeap := make(PassRatioHeap, 0, len(classes))
	for _, class := range classes {
		passRatioHeap = append(passRatioHeap, class)
	}
	heap.Init(&passRatioHeap)

	for extraStudents > 0 {
		class := heap.Pop(&passRatioHeap).([]int)
		class[0]++
		class[1]++
		extraStudents--

		heap.Push(&passRatioHeap, class)
	}

	var ret float64
	for _, class := range passRatioHeap {
		ret += float64(class[0]) / float64(class[1])
	}
	return ret / float64(len(classes))
}

func passRatioDifference(class []int) float64 {
	return (float64((class[0] + 1)) / float64((class[1] + 1))) - (float64((class[0])) / float64((class[1])))
}

type PassRatioHeap [][]int

func (h PassRatioHeap) Len() int      { return len(h) }
func (h PassRatioHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h PassRatioHeap) Less(i, j int) bool {
	return passRatioDifference(h[i]) > passRatioDifference(h[j])
}

func (h *PassRatioHeap) Push(x interface{}) {
	// Push and Pop use pointer receivers because they modify the slice's length,
	// not just its contents.
	*h = append(*h, x.([]int))
}

func (h *PassRatioHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}
```

### Statement

Here I messed up by assuming if a class had a lower pass ratio it would be the first to need extra students. Then I tried to maintain a queue where I would check against the current and next entry. Both of these assumptions were wrong to make. The max difference in pass ratio had nothing to do with the ordering. Using a queue does not enforce the value re-queued to be in the right place from an ordering perspective. In this way, a queue would be almost no different than iterating through all the values with a for loop for every extra student, similar to the brute force approach above.

The correct approach was to use a max heap of the difference in pass ratio. This way, the max effected class could be selected to receive an extra student every time, thus, maximizing the average pass ratio over all the classes.
