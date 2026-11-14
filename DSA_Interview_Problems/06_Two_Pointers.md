# Two Pointers – DSA Interview Practice

**Problems:** 2  
**Languages:** Java and Python

---

# 1. Pair Sum in Sorted Array

**Difficulty:** Easy  
**Pattern:** Two Pointers / Sorted Array

## Problem Statement

Given a sorted array and target, find two different indices whose values sum to the target.

## Input Format

```text
a = [1,2,4,6,10], target = 8
```

## Output Format

```text
[1,3]
```

## Constraints

- Array is sorted; indices must be different; return convention depends on platform.

## Example

```text
Input:  a = [1,2,4,6,10], target = 8
Output: [1,3]

Additional Example 2 Input:  a=[2,3,5,8], target=10
Additional Example 2 Output: [0,3]

Additional Example 3 Input:  a=[1,2,4], target=20
Additional Example 3 Output: [-1,-1]
```

## Approach

Place one pointer at each end of the sorted array. If the sum is too small, move the left pointer right; if the sum is too large, move the right pointer left. Return when the target sum is found.

## Iteration / Dry Run

1. left=0 (1), right=4 (10), sum=11>8; move right to 3.
2. 1+6=7<8; move left to 1.
3. 2+6=8; return [1,3].

## Java Solution

```java
class Solution {
    public int[] pairSum(int[] a, int target) {
        int l = 0, r = a.length - 1;
        while (l < r) {
            int sum = a[l] + a[r];
            if (sum == target) return new int[]{l, r};
            if (sum < target) l++;
            else r--;
        }
        return new int[]{-1, -1};
    }
}
```

## Python Solution

```python
def pair_sum(a, target):
    l, r = 0, len(a) - 1
    while l < r:
        total = a[l] + a[r]
        if total == target: return [l, r]
        if total < target: l += 1
        else: r -= 1
    return [-1, -1]
```

**Time:** `O(n)`  
**Space:** `O(1)`

---

# 2. Remove Duplicates from Sorted Array

**Difficulty:** Easy  
**Pattern:** Two Pointers / Read and Write

## Problem Statement

Remove duplicates in place from a sorted array so each value appears once; return the number of unique values.

## Input Format

```text
a = [0,0,1,1,1,2,2,3,3,4]
```

## Output Format

```text
5; first five values become [0,1,2,3,4]
```

## Constraints

- Array is sorted; modify in place; returned k identifies the valid prefix.

## Example

```text
Input:  a = [0,0,1,1,1,2,2,3,3,4]
Output: 5; first five values become [0,1,2,3,4]

Additional Example 2 Input:  a=[1,1,2]
Additional Example 2 Output: k=2; prefix=[1,2]

Additional Example 3 Input:  a=[1,2,3]
Additional Example 3 Output: k=3; prefix=[1,2,3]
```

## Approach

Use a `write` pointer to mark the next position for a new value. Scan from left to right and copy a value only when it differs from the last unique value written. The first `k` positions then contain the unique values.

## Iteration / Dry Run

1. Start write=1.
2. Read second 1; equals last written value, skip.
3. Read 2; copy to index 1, write=2.
4. Read next 2; skip.
5. Read 3; copy to index 2, write=3. Return k=3.

## Java Solution

```java
class Solution {
    public int removeDuplicates(int[] a) {
        if (a.length == 0) return 0;
        int write = 1;
        for (int read = 1; read < a.length; read++) {
            if (a[read] != a[write - 1])
                a[write++] = a[read];
        }
        return write;
    }
}
```

## Python Solution

```python
def remove_duplicates(a):
    if not a: return 0
    write = 1
    for read in range(1, len(a)):
        if a[read] != a[write - 1]:
            a[write] = a[read]
            write += 1
    return write
```

**Time:** `O(n)`  
**Space:** `O(1)`

---
