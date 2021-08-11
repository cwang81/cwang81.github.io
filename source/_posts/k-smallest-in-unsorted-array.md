---
title: K Smallest In Unsorted Array
date: 2021-08-08 18:40:08
categories: leetcode
tags: [pq, quick select]
---

### Clarification & Assumption:
    Input & Output (signature):
        i. input: int[] array, int k
        ii. output: int[], k smallest in ascending order
    For all the possible cases:
        input not sorted
        k >=0, k <= input.length
        array not null
        corner case: array == [], k == 0 → return empty array
<!-- more -->

### Result:
    high level: 
        1. bfs, maxHeap (less space than minHeap)
        2. quick select

	Queue<Integer> maxHeap;

	i = 0 to k-1 → maxHeap.offer(array[i]);

	i = k to array.length - 1:
		if smaller than maxHeap.peek();
			maxHeap.poll();
			maxHeap.offer(array[i]);

	int[] result;
	put all numbers from maxHeap to result;

	Complexity analysis:
		TC: O(k + (n-k)logk)
		SC: O(k)

### Test cases:
    i. Test corner case: length == 0, k == 0 → return empty array
    ii. Test general case: 9 1 2 7 8 5 3, k = 0/1/2/3/4, etc.

### 1. Priority Queue

``` java
public int[] kSmallest(int[] array, int k) {
	if (array.length == 0 || k == 0) {
		return new int[0];
    }
    Queue<Integer> maxHeap = new PriorityQueue<Integer>(k, (Integer i1, Integer i2) -> i1.equals(i2) ? 0 : i1 > i2 ? -1 : 1);
    for (int i = 0; i < array.length; i += 1) {
        if (i < k) {
            maxHeap.offer(array[i]);
        } else {
            if (array[i] < maxHeap.peek()) {
                maxHeap.poll();
                maxHeap.offer(array[i]);
            }
        }
    }
    int[] result = new int[k];
    for (int i = k - 1; i >= 0; i -= 1) {
        result[i] = maxHeap.poll();
    }
    return result;
}

```

### 2. Quick Select

    Complexity analysis:
        TC: O(n) = n + n/2 + n/4 + … + 1 
        worse case: O(n^2)	
        SC: O(1)

``` java
public int[] kSmallest(int[] array, int k) {
	if (array.length == 0 || k == 0) {
        return new int[0];
    }
    int left = 0;
    int right = array.length - 1;
    while (left < right) {
        int index = partition(array, left, right);
        if (index < k - 1) {
            left = index + 1;
        } else if (index > k - 1) {
            right = index - 1;
        } else {
            break;
        }
    }
    int[] result = Arrays.copyOf(array, k);
    Arrays.sort(result);
    return result;
}

private int partition(int[] array, int left, int right) {
	int pivotIndex = left + (int)(Math.random() * (right - left + 1));
	int pivot = array[pivotIndex];
	swap(array, pivotIndex, right);
	int i = left;
	int j = right - 1;
	while (i <= j) {
		if (array[i] < pivot) {
			i++;
        } else if (array[j] >= pivot) {
            j--;
        } else {
            swap(array, i++, j--);
        }
    }
    swap(array, i, right);
    return i;
}

private void swap(int[] array, int index1, int index2) {
	int tmp = array[index1];
	array[index1] = array[index2];
	array[index2] = tmp;
}
```