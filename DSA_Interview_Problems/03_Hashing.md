# Hashing – DSA Interview Practice

**Problems:** 3  
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
2. At index 0, value 2 needs 7; it is absent, so store `2 → 0`.
3. At index 1, value 7 needs 2; 2 is present at index 0.
4. Return `[0,1]`.

## Java Solution

```java
import java.util.*;
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> seen = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int need = target - nums[i];
            if (seen.containsKey(need)) {
                return new int[]{seen.get(need), i};
            }
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

# 2. Frequency Count

**Difficulty:** Easy  
**Pattern:** Hashing / Frequency Map

## Problem Statement

Count how many times each distinct value appears in an array.

## Input Format

```text
a = [1,2,2,3,1,2]
```

## Output Format

```text
{1:2, 2:3, 3:1}
```

## Constraints

- 0 <= a.length <= 10^5; values may be negative; output ordering depends on the required format.

## Example

```text
Input:  a = [1,2,2,3,1,2]
Output: {1:2, 2:3, 3:1}

Additional Example 2 Input:  a=[4,4,4]
Additional Example 2 Output: {4:3}

Additional Example 3 Input:  a=[-1,2,-1]
Additional Example 3 Output: {-1:2, 2:1}
```

## Approach

Use a HashMap where each key is an array value and each value is its frequency. For every element, increase its existing count or initialize the count to one.

## Iteration / Dry Run

1. Empty map.
2. Read 1 → {1:1}.
3. Read 2 twice → {1:1,2:2}.
4. Read 3 → {1:1,2:2,3:1}.
5. Read 1 → {1:2,2:2,3:1}.
6. Read 2 → {1:2,2:3,3:1}.

## Java Solution

```java
import java.util.*;
class Solution {
    public Map<Integer, Integer> frequency(int[] a) {
        Map<Integer, Integer> freq = new LinkedHashMap<>();
        for (int x : a) freq.put(x, freq.getOrDefault(x, 0) + 1);
        return freq;
    }
}
```

## Python Solution

```python
from collections import Counter
def frequency(a):
    return Counter(a)
```

**Time:** `O(n) average`  
**Space:** `O(k), where k is the number of distinct values`

---

# 3. Duplicate Elements

**Difficulty:** Easy  
**Pattern:** Hashing / Hash Set

## Problem Statement

Determine whether an array contains at least one value that appears more than once.

## Input Format

```text
a = [1,2,3,1]
```

## Output Format

```text
true
```

## Constraints

- 0 <= a.length <= 10^5; values may be negative.

## Example

```text
Input:  a = [1,2,3,1]
Output: true

Additional Example 2 Input:  a=[1,2,3]
Additional Example 2 Output: false

Additional Example 3 Input:  a=[0,0]
Additional Example 3 Output: true
```

## Approach

Use a HashSet to remember values already visited. If adding the current value fails because it is already present, a duplicate exists. If the scan finishes without a repeated value, return false.

## Iteration / Dry Run

1. Start with an empty set.
2. Add 1, then 2, then 3.
3. Read the last 1; it is already in the set.
4. Return true.

## Java Solution

```java
import java.util.*;
class Solution {
    public boolean containsDuplicate(int[] a) {
        Set<Integer> seen = new HashSet<>();
        for (int x : a) if (!seen.add(x)) return true;
        return false;
    }
}
```

## Python Solution

```python
def contains_duplicate(a):
    seen = set()
    for x in a:
        if x in seen: return True
        seen.add(x)
    return False
```

**Time:** `O(n) average`  
**Space:** `O(n)`

---
