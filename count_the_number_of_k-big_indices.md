# Count the Number of K-Big Indices

## Metadata

- **Date Solved:** January 7, 2024 8:39 PM
- **Difficulty:** Hard
- **Link:** https://leetcode.com/problems/count-the-number-of-k-big-indices/description/?envType=study-plan-v2&envId=amazon-spring-23-high-frequency
- **Algorithm:** binary index tree, heap, two pass
- **Type:** array

## Problem Statement

You are given a 0-indexed integer array nums and a positive integer k.
We call an index i k-big if the following conditions are satisfied:

- There exist at least k different indices idx1 such that idx1 < iand nums[idx1] < nums[i].
- There exist at least k different indices idx2 such that idx2 > iand nums[idx2] < nums[i].

Return the number of k-big indices.

## Constraints

- 1 <= nums.length <= 105
- 1 <= nums[i], k <= nums.length

## Solution

- **Status:** Looked At Discuss

### Solution Notes

I first tried to make a monotonic stack work but there were cases that broke the approach. I was unable to think of others, though BIT and heap should have been a clue given the 'k-' nature of the problem.

BIT can be used here because we are able to construct the tree with the array values as the index keys and the value at the keys as the frequency (prefix sum). In this way, we could count the number of entries less than the current easily and accumulate as we progress. A major reason why BIT works here in this way is because the numbers in nums are all between 1 and len(nums). Since this is the case when we insert keys we can iterate from key till length inclusive. This ensures all test cases are met.

If the above wasn't a constraint, a max heap would do the trick. In this case, we would populate the max heap with the first/last k elements and for ever element after we would see if the top of the heap is less than the cur. If so we account for that and add it to the heap. For every addition to the heap we ensure we have at most k elements and pop off any http://extra.In In this case we could use a map to keep track of valid indices given the set of numbers is much more broad than the constraints given. 


### BIT

```go
package main

type BIT [100001]int

func (b *BIT) insert(k, v int, size int) {
	for ; k <= size; k += (k & (-k)) {
		b[k] += v
	}
}

func (b *BIT) query(k int) int {
	res := 0
	for ; k > 0; k -= (k & (-k)) {
		res += b[k]
	}
	return res
}

func kBigIndices(nums []int, k int) int {
	size := len(nums)
	pre, post := BIT{}, BIT{}
	for i := 0; i < len(nums); i++ {
		post.insert(nums[i], 1, size)
	}
	ans := 0
	for i := 0; i < size; i++ {
		num := nums[i]
		if pre.query(num-1) >= k && post.query(num-1) >= k {
			ans++
		}
		pre.insert(num, 1, size)
		post.insert(num, -1, size)
	}
	return ans
}
```

### MaxHeap

```go
package main

import "container/heap"

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h IntHeap) Peek() interface{}  { return h[0] }
func (h *IntHeap) Push(o interface{}) {
	*h = append(*h, o.(int))
}
func (h *IntHeap) Pop() interface{} {
	tmp := *h
	l := len(tmp)
	x := tmp[l-1]
	*h = tmp[0 : l-1]
	return x
}

func kBigIndices(nums []int, k int) int {
	h := IntHeap{}
	h = append(h, nums[0:k]...)
	heap.Init(&h)

	numCounter := make([]int, len(nums)+1)

	for i := k; i < len(nums); i++ {
		if h.Peek().(int) < nums[i] {
			numCounter[i]++
		}
		heap.Push(&h, nums[i])
		for h.Len() > k {
			heap.Pop(&h)
		}
	}

	h = IntHeap{}
	h = append(h, nums[len(nums)-k:len(nums)]...)
	heap.Init(&h)

	ans := 0
	for i := len(nums) - k - 1; i >= 0; i-- {
		if h.Peek().(int) < nums[i] && numCounter[i] > 0 {
			ans++
		}

		heap.Push(&h, nums[i])
		for h.Len() > k {
			heap.Pop(&h)
		}
	}
	return ans
}
```
