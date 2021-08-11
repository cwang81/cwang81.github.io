---
title: LC 90. SubSets II
date: 2021-08-09 22:00:08
categories: leetcode
tags: [dfs]
---

https://leetcode.com/problems/subsets-ii/

### Clarification & Assumption:
	Description: given an integer array, find all possible subsets
		         e.g. [1,2,2] -> [1], [2], [1,2], [2,2], [1,2,2]
	Input & Output:
		i. input: String set
		ii. output: List<String>
	For all the possible cases:
		there could be duplicate characters
<!-- more -->

### Result:
	high level: DFS

	i. sort the characters in ascending order
   		to make the duplicate character next to each other
	ii. DFS backtracking

	abb  →  '', a, b, ab, bb, abb
		
			               ''
		         /                   \
    0		    a                    ''
 		      /     \               /      \
    1	     ab      a            b          ''
	       /    \       \        /   \          \ 
    2	  abb   ab      a      bb     b         ''

	unliked the subset i problem, here it contains duplicate results
	one naive approach is to use a HashMap and deduplicate the results when we get one result
	but the TC and SC is too large

	here we can first sort the elements in ascending order
	so that the same elements are grouped together

	then we can jump over the duplicated element when we choose to NOT ADD it
	e.g. for [a,b,b], when we have already added a->b
	     we should not add another a->b, but jump over the same elements, i.e. b

	Complexity analysis:
		TC: O(2^n)
		SC: O(n)

### Test cases:
	i. Test corner case:  null
	ii. Test general case:  12, 122, 12222, 212223333

```java
    public List<List<Integer>> subsetsWithDup(int[] array) {
        List<List<Integer>> result = new ArrayList<>();
        if (array == null || array.length == 0) {
            return result;
        }
        List<Integer> list = new ArrayList<>();
        Arrays.sort(array);
        helper(array, 0, list, result);
        return result;
    }
    
    private void helper(int[] array, int index, List<Integer> list, List<List<Integer>> result) {
        // base case
        if (index == array.length) {
            result.add(new ArrayList<>(list));
            return;
        }
        
        list.add(array[index]);
        helper(array, index + 1, list, result);
        list.remove(list.size() - 1);
        
        int i = index + 1;
        while (i < array.length && array[i] == array[index]) {
            i++;
        }
        helper(array, i, list, result);
    }
```