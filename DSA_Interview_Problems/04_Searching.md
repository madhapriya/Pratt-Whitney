# Searching – DSA Interview Practice

**Problems:** 2  
**Languages:** Java and Python

---

# 1. Binary Search

**Difficulty:** Easy  
**Pattern:** Searching / Binary Search

## Problem Statement

Given a sorted array and a target, return its index or -1 if it is absent.

## Input Format

```text
a = [-1,0,3,5,9,12], target = 9
```

## Output Format

```text
4
```

## Constraints

- Array is sorted in ascending order; 0 <= length <= 10^4.

## Example

```text
Input:  a = [-1,0,3,5,9,12], target = 9
Output: 4

Additional Example 2 Input:  a=[1,3,5], target=2
Additional Example 2 Output: -1

Additional Example 3 Input:  a=[2], target=2
Additional Example 3 Output: 0
```

## Approach

Because the array is sorted, compare the target with the middle element. If the target is larger, discard the left half; if it is smaller, discard the right half. Continue until the target is found or the search range becomes empty.

## Iteration / Dry Run

1. left=0,right=5,mid=2; value 3<9, so left=3.
2. mid=4; value 9 matches.
3. Return index 4.

## Java Solution

```java
class Solution {
    public int search(int[] a, int target) {
        int l = 0, r = a.length - 1;
        while (l <= r) {
            int m = l + (r - l) / 2;
            if (a[m] == target) return m;
            if (a[m] < target) l = m + 1;
            else r = m - 1;
        }
        return -1;
    }
}
```

## Python Solution

```python
def search(a, target):
    l, r = 0, len(a) - 1
    while l <= r:
        m = l + (r - l) // 2
        if a[m] == target: return m
        if a[m] < target: l = m + 1
        else: r = m - 1
    return -1
```

**Time:** `O(log n)`  
**Space:** `O(1)`

---

# 2. First and Last Position

**Difficulty:** Medium  
**Pattern:** Searching / Boundary Binary Search

## Problem Statement

In a sorted array, return the first and last index of a target value, or [-1,-1] if it is absent.

## Input Format

```text
a = [5,7,7,8,8,10], target = 8
```

## Output Format

```text
[3,4]
```

## Constraints

- 0 <= length <= 10^5; array is sorted in non-decreasing order.

## Example

```text
Input:  a = [5,7,7,8,8,10], target = 8
Output: [3,4]

Additional Example 2 Input:  a=[1,2,2,2,3], target=2
Additional Example 2 Output: [1,3]

Additional Example 3 Input:  a=[1,2,3], target=4
Additional Example 3 Output: [-1,-1]
```

## Approach

Run binary search twice. The first search continues toward the left after finding the target; the second continues toward the right. This finds both boundaries in logarithmic time.

## Iteration / Dry Run

First search: mid=2 gives 7<8, move right; mid=4 gives 8, save 4 and continue left; mid=3 gives 8, save 3 and continue left. First=3. Last search moves right after a match and finishes at index
4. Return [3,4].

## Java Solution

```java
class Solution {
    public int[] searchRange(int[] a, int target) {
        return new int[]{bound(a, target, true), bound(a, target, false)};
    }
    private int bound(int[] a, int target, boolean first) {
        int l = 0, r = a.length - 1, ans = -1;
        while (l <= r) {
            int m = l + (r - l) / 2;
            if (a[m] == target) {
                ans = m;
                if (first) r = m - 1;
                else l = m + 1;
            } else if (a[m] < target) l = m + 1;
            else r = m - 1;
        }
        return ans;
    }
}
```

## Python Solution

```python
def search_range(a, target):
    def bound(first):
        l, r, ans = 0, len(a) - 1, -1
        while l <= r:
            m = (l + r) // 2
            if a[m] == target:
                ans = m
                if first: r = m - 1
                else: l = m + 1
            elif a[m] < target: l = m + 1
            else: r = m - 1
        return ans
    return [bound(True), bound(False)]
```

**Time:** `O(log n)`  
**Space:** `O(1)`

---
