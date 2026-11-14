# Arrays – DSA Interview Practice

**Problems:** 4  
**Languages:** Java and Python

---

# 1. Two Sum

**Difficulty:** Easy  
**Pattern:** Array / Hash Map

## Problem Statement

Given an integer array nums and an integer target, return the indices of two distinct elements whose values add up to target.

## Input Format

```text
nums = [2,7,11,15], target = 9
```

## Output Format

```text
[0,1]
```

## Constraints

- 2 <= nums.length <= 10^4; values and target may be negative; exactly one solution is commonly guaranteed by the interview version.

## Example

```text
Input:  nums = [2,7,11,15], target = 9
Output: [0,1]

Additional Example 2 Input:  nums=[2,5,5,11], target=10
Additional Example 2 Output: [1,2]

Additional Example 3 Input:  nums=[3,3], target=6
Additional Example 3 Output: [0,1]
```

## Approach

For each element, calculate its complement as `target - nums[i]`. If the complement is already stored in a HashMap, return the stored index and the current index. Otherwise, store the current value and its index. Checking before inserting prevents using the same element twice.

## Iteration / Dry Run

1. Start with an empty map.
2. At index 0, value=2; need=9−2=7, not found, so store 2→0.
3. At index 1, value=7; need=2, found at index 0.
4. Return [0,1].

## Java Solution

```java
import java.util.*;
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> seen = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int need = target - nums[i];
            if (seen.containsKey(need))
                return new int[]{seen.get(need), i};
            seen.put(nums[i], i);
        }
        return new int[0];
    }
}
```

## Python Solution

```python
def two_sum(nums, target):
    seen = {}
    for i, value in enumerate(nums):
        need = target - value
        if need in seen:
            return [seen[need], i]
        seen[value] = i
    return []
```

**Time:** `O(n) average`  
**Space:** `O(n)`

---

# 2. Move Zeroes

**Difficulty:** Easy  
**Pattern:** Array / Two Pointers

## Problem Statement

Move every zero in an integer array to the end while preserving the relative order of non-zero elements. Modify the array in place.

## Input Format

```text
nums = [0,1,0,3,12]
```

## Output Format

```text
[1,3,12,0,0]
```

## Constraints

- 1 <= nums.length <= 10^4; values may be zero or non-zero.

## Example

```text
Input:  nums = [0,1,0,3,12]
Output: [1,3,12,0,0]

Additional Example 2 Input:  nums=[0,0,0,2]
Additional Example 2 Output: [2,0,0,0]

Additional Example 3 Input:  nums=[1,2,3]
Additional Example 3 Output: [1,2,3]
```

## Approach

Maintain a `write` pointer for the next position where a non-zero value belongs. Scan the array from left to right. When a non-zero value is found, move it to `write` and advance `write`. This preserves the order of non-zero values and modifies the array in place.

## Iteration / Dry Run

1. Set write=0.
2. Read 0 at index 0: skip.
3. Read 1 at index 1: move it to index 0; write=1.
4. Skip 0 at index 2.
5. Move 3 to index 1; write=2.
6. Move 12 to index 2; write=3. Result=[1,3,12,0,0].

## Java Solution

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int write = 0;
        for (int read = 0; read < nums.length; read++) {
            if (nums[read] != 0) {
                int t = nums[write];
                nums[write] = nums[read];
                nums[read] = t;
                write++;
            }
        }
    }
}
```

## Python Solution

```python
def move_zeroes(nums):
    write = 0
    for read in range(len(nums)):
        if nums[read] != 0:
            nums[write], nums[read] = nums[read], nums[write]
            write += 1
```

**Time:** `O(n)`  
**Space:** `O(1)`

---

# 3. Maximum Subarray

**Difficulty:** Medium  
**Pattern:** Array / Kadane's Algorithm

## Problem Statement

Given an integer array, return the largest possible sum of a non-empty contiguous subarray.

## Input Format

```text
nums = [-2,1,-3,4,-1,2,1,-5,4]
```

## Output Format

```text
6
```

## Constraints

- 1 <= nums.length <= 10^5; -10^4 <= nums[i] <= 10^4.

## Example

```text
Input:  nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6

Additional Example 2 Input:  nums=[1]
Additional Example 2 Output: 1

Additional Example 3 Input:  nums=[-3,-2,-5]
Additional Example 3 Output: -2
```

## Approach

Use Kadane's algorithm. At each element, decide whether to start a new subarray at that element or extend the current subarray. Update the best sum after every element. Initializing both values with the first array element correctly handles arrays containing only negative numbers.

## Iteration / Dry Run

1. Initialize current=best=-2.
2. Read 1: current=max(1,-1)=1; best=1.
3. Read -3: current=-2; best=1.
4. Read 4: current=4; best=4.
5. Read -1: current=3.
6. Read 2: current=5; read 1: current=6.
7. Read -5: current=1; read 4: current=5. Final best=6.

## Java Solution

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int current = nums[0], best = nums[0];
        for (int i = 1; i < nums.length; i++) {
            current = Math.max(nums[i], current + nums[i]);
            best = Math.max(best, current);
        }
        return best;
    }
}
```

## Python Solution

```python
def max_sub_array(nums):
    current = best = nums[0]
    for x in nums[1:]:
        current = max(x, current + x)
        best = max(best, current)
    return best
```

**Time:** `O(n)`  
**Space:** `O(1)`

---

# 4. Missing Number

**Difficulty:** Easy  
**Pattern:** Array / Mathematical Formula

## Problem Statement

An array contains n distinct numbers from the range 0 through n. Return the one number that is missing.

## Input Format

```text
nums = [3,0,1]
```

## Output Format

```text
2
```

## Constraints

- n == nums.length; all values are distinct and lie in [0,n].

## Example

```text
Input:  nums = [3,0,1]
Output: 2

Additional Example 2 Input:  nums=[0,1]
Additional Example 2 Output: 2

Additional Example 3 Input:  nums=[9,6,4,2,3,5,7,0,1]
Additional Example 3 Output: 8
```

## Approach

Calculate the expected sum of all values from 0 through n using `n * (n + 1) / 2`. Subtract the actual array sum from the expected sum. The remaining value is the missing number.

## Iteration / Dry Run

1. n=3; expected sum=3×4/2=6.
2. Actual sum=3+0+1=4.
3. Missing=6−4=2.

## Java Solution

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int expected = n * (n + 1) / 2;
        int actual = 0;
        for (int x : nums) actual += x;
        return expected - actual;
    }
}
```

## Python Solution

```python
def missing_number(nums):
    n = len(nums)
    return n * (n + 1) // 2 - sum(nums)
```

**Time:** `O(n)`  
**Space:** `O(1)`

---
