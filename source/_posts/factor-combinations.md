---
title: LC 254. Factor Combinations
date: 2021-08-10 21:30:54
categories: leetcode
tags: [dfs]
---

https://leetcode.com/problems/factor-combinations/

### Clarification & Assumption
    Description: Giving a number n, return all possible combinations of its factors
    
    input: int n
    output: List<List<Integer>>
    for all the possible cases:
        e.g. n = 1, return []
             n = 12, return [[2,6], [3,4], [2,2,3]]
<!-- more -->

### Result
    high level: dfs

    there are 2 ways to do dfs for this problem
    1. add one factor at each level
                              12
                    /      |       |      \
        2         0        2      2x2    2x2x2   
                /  \     /   \     |            
        3      0    3   0     3    3    
            /  \
        4   ...

        we can add one factor at each level
        as only using one factor at each level, duplicate results are avoided

        # of levels: sqrt(n) in maximum
        each level represent: # of factors added at current level
        # of states: n / level

        TC: O(logn^sqrt(n))
        SC: O(logn^sqrt(n))

    2. add multiple factors at each level
                              12
                    /      |       |      \
                   2       3       4       6  
                /  \       |                   
               2    3      4           
        
        we can also vary the factor at each level
        but the deduplicate, we need to keep the next level factors larger than the current one we choose
        e.g. otherwise, such factor combination could happen: 3->2->2, which is the same as 2->2->3

        # of levels: logn
        each level represent: which factor to be added
        # of states: sqrt(n)

        TC: O(sqrt(n)^logn)
        SC: O(sqrt(n)^logn)

    here I chose the 2nd approach, because if n is large, logn < sqrt(n)
    here is an image showing the trend:
{% img /picture/leetcode/logn_vs_sqrtn.png %}

### Test cases:
    corner case: <= 1 -> return empty list
    general cases: 2, 4, 12, 24, 37, 55, 101, 1000000

```java
    public List<List<Integer>> getFactors(int n) {
        List<List<Integer>> res = new ArrayList<>();
        dfs(n, 2, new ArrayList<>(), res);
        return res;
    }
    
    private void dfs(int n, int factor, List<Integer> list, List<List<Integer>> res) {
        // base case
        if (n <= 1) return;
        
        for (int i = factor; i * i <= n; i++) {
            if (n % i == 0) {
                list.add(i);
                list.add(n / i);
                res.add(new ArrayList<>(list));
                list.remove(list.size() - 1);
                dfs(n / i, i, list, res);
                list.remove(list.size() - 1);
            }
        }
    }
```