# Find All Possible Recipes from Given Supplies

## Metadata

- **Date Solved:** November 21, 2023 12:20 AM
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/find-all-possible-recipes-from-given-supplies/description/
- **Algorithm:** bfs, topological sort
- **Type:** graph

## Problem Statement

You have information about n different recipes. You are given a string array recipes and a 2D string array ingredients. The ith recipe has the name recipes[i], and you can create it if you have all the needed ingredients from ingredients[i]. Ingredients to a recipe may need to be created from other recipes, i.e., ingredients[i] may contain a string that is in recipes.
You are also given a string array supplies containing all the ingredients that you initially have, and you have an infinite supply of all of them.

Return a list of all the recipes that you can create. You may return the answer in any order.

Note that two recipes may contain each other in their ingredients.

## Constraints

- n == recipes.length == ingredients.length
- 1 <= n <= 100
- 1 <= ingredients[i].length, supplies.length <= 100
- 1 <= recipes[i].length, ingredients[i][j].length, supplies[k].length <= 10
- recipes[i], ingredients[i][j], and supplies[k]consist only of lowercase English letters.
- All the values of recipes and supplies combined are unique.
- Each ingredients[i] does not contain any duplicate values.

## Solution

- **Status:** Solved On Own


```go
package main

func findAllRecipes(recipes []string, ingredients [][]string, supplies []string) []string {
	adjList := make(map[string][]string)
	ref := make(map[string]int)
	for i, greets := range ingredients {
		for _, g := range greets {
			adjList[g] = append(adjList[g], recipes[i])
			ref[recipes[i]]++
		}
	}

	ans := []string{}
	for len(supplies) > 0 {
		cur := supplies[0]
		supplies = supplies[1:]
		for _, n := range adjList[cur] {
			ref[n]--
			if ref[n] == 0 {
				ans = append(ans, n)
				supplies = append(supplies, n)
			}
		}
	}
	return ans
}
```
