# Count Good Meals

## Metadata

- **Date Solved:** December 15, 2023 3:24 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/count-good-meals/description/
- **Algorithm:** hashmap, two pointer
- **Type:** array

## Problem Statement

A good meal is a meal that contains exactly two different food items with a sum of deliciousness equal to a power of two.

You can pick any two different foods to make a good meal.
Given an array of integers deliciousness where deliciousness[i] is the deliciousness of the ith item of food, return the number of different good meals you can make from this list modulo 109 + 7.

Note that items with different indices are considered different even if they have the same deliciousness value.

## Constraints

- 1 <= deliciousness.length <= 105
- 0 <= deliciousness[i] <= 220

## Solution

- **Status:** Looked At Discuss

### Solution Notes

My first attempt I was trying to do too much. Although the right intuitions are there the implementation did not pass a basic test case. The reason here being in having to account for which values we encountered yet or not, if one of the values we encountered was repeated, it would be skipped given it was used for a lower number. Example being [0, 1, 1]. 0+1 == 1 (2^0). Then when we try for 1+1 = 2 (2^1), we would see that both 1 and 1 were encountered already and not count them.

After looking at solutions one key thing I missed was around not preloading the hash with entires and their counts. When pre-loading you have to take into account values seen and accidentally hitting the same value twice A better solution seemed to be to iterate through and add entries as you encounter them. In this way, 


```go
package main

import "math"

var mod = 1000000007

func countPairs(deliciousness []int) int {
	ans := 0
	vals := make(map[int]int)
	for _, d := range deliciousness {
		for i := int(math.Pow(2, 0)); i < int(math.Pow(2, 22)); i += i {
			if v, exists := vals[i-d]; exists {
				ans = (ans + v) % mod
			}
		}
		vals[d]++
	}
	return ans
}
```

### First Attempt (Failed)

```go
package main

import (
	"math"
	"sort"
)

var mod = 1000000007

func countPairs(deliciousness []int) int {
	counts := make(map[int]int)
	unique := []int{}
	for _, d := range deliciousness {
		if _, exist := counts[d]; !exist {
			unique = append(unique, d)
		}
		counts[d]++
	}

	sort.Ints(unique)

	ans := 0
	var start float64
	encountered := make(map[int]bool)
	for j := 0; j < len(unique); j++ {
		for i := int(math.Pow(2, start)); i < int(math.Pow(2, 22)); i += i {
			n1 := unique[j]
			n1Count := counts[n1]
			n2 := i - n1

			if encountered[n1] && encountered[n2] {
				continue
			}
			if n2 < 0 {
				start++
				continue
			}

			n2Count, exists := counts[n2]
			if !exists {
				continue
			}

			countOf := n1Count
			if n1 != n2 {
				countOf = n1Count * n2Count
			} else {
				countOf = (n1Count * (n1Count - 1)) / 2
			}

			//fmt.Println(i, n1, n2, n1Count, n2Count, countOf)

			ans = ans + (countOf % mod)
			encountered[n1], encountered[n2] = true, true
		}
		if len(encountered) == len(unique) {
			break
		}
	}
	return ans
}
```
