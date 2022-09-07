# Remove K Digits

## Metadata

- **Date Solved:** September 6, 2022 10:41 PM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/remove-k-digits/
- **Algorithm:** greedy, monotonic stack
- **Type:** number theory, string

## Problem Statement

Given string num representing a non-negative integer num, and an integer k, return the smallest possible integer after removing k digits from num.

## Constraints

- 1 <= k <= num.length <= 105
- num consists of only digits.
- num does not have any leading zeros except for the zero itself.

## Solution

- **Status:** Solved On Own


```go
package main

func removeKdigits(num string, k int) string {
	if k >= len(num) {
		return "0"
	}

	monoStack := make([]int, 0, len(num))
	for i := range num {
		for len(monoStack) > 0 && k > 0 {
			if num[monoStack[len(monoStack)-1]] <= num[i] {
				break
			}
			monoStack = monoStack[0 : len(monoStack)-1]
			k--
		}
		monoStack = append(monoStack, i)
	}

	if k > 0 {
		monoStack = monoStack[0 : len(monoStack)-k]
	}

	ans := make([]byte, 0, len(monoStack))
	for _, i := range monoStack {
		if len(ans) == 0 && num[i] == '0' {
			continue
		}
		ans = append(ans, num[i])
	}

	if len(ans) > 0 {
		return string(ans)
	}
	return "0"
}
```

```go
package main

func removeKdigits(num string, k int) string {
	if k >= len(num) {
		return "0"
	}

	var ret []byte
	ret = append(ret, num[0])
	var i int
	for i = 1; i < len(num) && k > 0; i++ {
		for k > 0 && len(ret) > 0 {
			if ret[len(ret)-1] <= num[i] {
				break
			}
			ret = ret[0 : len(ret)-1]
			k--
		}
		ret = append(ret, num[i])
	}

	if i < len(num) {
		ret = append(ret, num[i:len(num)]...)
	} else if k > 0 {
		ret = ret[0 : len(ret)-k]
	}

	for len(ret) > 0 && ret[0] == '0' {
		ret = ret[1:len(ret)]
	}
	if len(ret) == 0 {
		return "0"
	}
	return string(ret)
}
```
