# Sliding Window – DSA Interview Practice

**Problems:** 2  
**Languages:** Java and Python

---

# 1. Maximum Sum Subarray of Size K

**Difficulty:** Easy  
**Pattern:** Sliding Window / Fixed Size

## Problem Statement

Find the maximum sum among all contiguous subarrays containing exactly k elements.

## Input Format

```text
a = [2,1,5,1,3,2], k = 3
```

## Output Format

```text
9
```

## Constraints

- 1 <= k <= a.length; values may be negative.

## Example

```text
Input:  a = [2,1,5,1,3,2], k = 3
Output: 9

Additional Example 2 Input:  a=[1,4,2,10,2,3], k=2
Additional Example 2 Output: 12

Additional Example 3 Input:  a=[-5,-2,-3], k=2
Additional Example 3 Output: -5
```

## Approach

Calculate the sum of the first k elements. For each next window, subtract the element that leaves the window and add the new element that enters it. Track the maximum sum.

## Iteration / Dry Run

1. First window sum=8.
2. Slide: subtract outgoing 2 and add incoming 1 → 7.
3. Slide: subtract 1 and add 3 → 9.
4. Slide: subtract 5 and add 2 → 6.
5. Best remains 9.

## Java Solution

```java
class Solution {
    public int maxSum(int[] a, int k) {
        int sum = 0;
        for (int i = 0; i < k; i++) sum += a[i];
        int best = sum;
        for (int right = k; right < a.length; right++) {
            sum += a[right] - a[right - k];
            best = Math.max(best, sum);
        }
        return best;
    }
}
```

## Python Solution

```python
def max_sum(a, k):
    window = sum(a[:k])
    best = window
    for right in range(k, len(a)):
        window += a[right] - a[right-k]
        best = max(best, window)
    return best
```

**Time:** `O(n)`  
**Space:** `O(1)`

---

# 2. Longest Substring Without Repeating Characters

**Difficulty:** Medium  
**Pattern:** Sliding Window / Variable Size

## Problem Statement

Return the length of the longest substring containing no repeated characters.

## Input Format

```text
s = 'abcabcbb'
```

## Output Format

```text
3
```

## Constraints

- 0 <= s.length <= 5 * 10^4; characters may include letters, digits, symbols, and spaces.

## Example

```text
Input:  s = 'abcabcbb'
Output: 3

Additional Example 2 Input:  s='bbbbb'
Additional Example 2 Output: 1

Additional Example 3 Input:  s='pwwkew'
Additional Example 3 Output: 3
```

## Approach

Maintain a sliding window with no repeated characters and a map of each character's most recent index. When a repeated character appears inside the window, move the left boundary just past its previous occurrence. Track the maximum window length.

## Iteration / Dry Run

1. Read a,b,c: window [0,2], best=3.
2. Read a at index 3; previous a was 0, so left=1.
3. Read b at 4; previous b=1, so left=2.
4. Read c at 5; previous c=2, so left=3.
5. Read b at 6; previous b=4, so left=5.
6. Read b at 7; left=7. Best remains 3.

## Java Solution

```java
import java.util.*;
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> last = new HashMap<>();
        int left = 0, best = 0;
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            if (last.containsKey(c))
                left = Math.max(left, last.get(c) + 1);
            last.put(c, right);
            best = Math.max(best, right - left + 1);
        }
        return best;
    }
}
```

## Python Solution

```python
def longest_unique(s):
    last = {}
    left = best = 0
    for right, c in enumerate(s):
        if c in last:
            left = max(left, last[c] + 1)
        last[c] = right
        best = max(best, right-left+1)
    return best
```

**Time:** `O(n) average`  
**Space:** `O(k), where k is the number of distinct characters`

---
