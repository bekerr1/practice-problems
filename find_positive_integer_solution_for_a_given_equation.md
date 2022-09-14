# Find Positive Integer Solution for a Given Equation

## Metadata

- **Date Solved:** September 13, 2022 10:02 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-positive-integer-solution-for-a-given-equation/
- **Algorithm:** binary search, dynamic programming, two pointer
- **Type:** math

## Problem Statement

Given a callable function f(x, y) with a hidden formula and a value z, reverse engineer the formula and return all positive integer pairs x and y where f(x,y) == z. You may return the pairs in any order.
While the exact formula is hidden, the function is monotonically increasing, i.e.:
- f(x, y) < f(x + 1, y)
- f(x, y) < f(x, y + 1)

## Constraints

- 1 <= function_id <= 9
- 1 <= z <= 100
- It is guaranteed that the solutions of f(x, y) == z will be in the range 1 <= x, y <= 1000.
- It is also guaranteed that f(x, y) will fit in 32 bit signed integer if 1 <= x, y <= 1000.

## Solution

- **Status:** Solved On Own

### Solution Notes

x, y can be between 1 and 1000. Iterating through these values is not such an issue. Additionally, notice finding f(x, y) == z requires iterating through the scale of 1..1000. This can be accomplished in several different ways


### DP (worst solution)

```go
package main

var memo [1000][1000]int

func findSolution(customFunction func(int, int) int, z int) [][]int {
	memo = [1000][1000]int{}
	ret := [][]int{}
	findSolutionUtil(1, 1, &ret, customFunction, z)
	return ret
}

func findSolutionUtil(x, y int, ret *[][]int, customFunction func(int, int) int, z int) {
	fxy := customFunction(x, y)
	saved := memo[x][y]
	if fxy == z && saved != fxy {
		*ret = append(*ret, []int{x, y})
		memo[x][y] = fxy
		return
	}

	if fxy > z {
		return
	}

	if memo[x][y] != 0 {
		return
	} else {
		memo[x][y] = fxy
		findSolutionUtil(x+1, y, ret, customFunction, z)
		findSolutionUtil(x, y+1, ret, customFunction, z)
	}

	return
}
```

### Binary Search

```go
package main

func findSolution(customFunction func(int, int) int, z int) [][]int {
	ret := [][]int{}
	for x := 1; x <= 1000; x++ {
		xy := findSolutionUtil(x, customFunction, z)
		if len(xy) == 2 {
			ret = append(ret, xy)
		}
	}
	return ret
}

func findSolutionUtil(x int, fn func(int, int) int, z int) []int {
	low, high := 1, 1000
	for low <= high {
		mid := low + (high-low)/2
		out := fn(x, mid)
		if out > z {
			high = mid - 1
		} else if out < z {
			low = mid + 1
		} else {
			return []int{x, mid}
		}
	}
	return []int{}
}
```

### Two Pointer

```go
package main

func findSolution(customFunction func(int, int) int, z int) [][]int {
	x, y := 1, 1000
	ret := [][]int{}

	for x <= 1000 && y >= 1 {
		fxy := customFunction(x, y)
		if fxy > z {
			y--
		} else if fxy < z {
			x++
		} else {
			ret = append(ret, []int{x, y})
			if x < 1000 {
				x++
			} else {
				y--
			}
		}
	}
	return ret
}
```
