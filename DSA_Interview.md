# DSA Interview Preparation – Easy and Optimal Java + Python Solutions

## Contents
1. Arrays · 2. Strings · 3. Hashing · 4. Searching · 5. Sorting · 6. Two Pointers · 7. Sliding Window · 8. Linked Lists · 9. Stack · 10. Queue · 11. Recursion · 12. Trees · 13. Pattern cheat sheet · 14. Complexity reference

---

# 1. ARRAYS

## 1.1 Two Sum

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** For each value, look for its complement (target - value)

### 1. Problem statement
Given an integer array nums and an integer target, return the indices of two distinct elements whose values add up to target.

### 2. Input and output format
- **Input:** `nums = [2,7,11,15], target = 9`
- **Output:** `[0,1]`

### 3. Constraints
2 <= nums.length <= 10^4; values and target may be negative; exactly one solution is commonly guaranteed by the interview version.

### 4. Examples
- **Example:** `nums = [3,2,4], target = 6 → [1,2]`
- **Additional Example 2:** Input `nums=[2,5,5,11], target=10` → Output `[1,2]`
- **Additional Example 3:** Input `nums=[3,3], target=6` → Output `[0,1]`


### 5. Pattern identification — how to recognize the problem
For each value, look for its complement (target - value). Use brute-force pair checking first; use a HashMap when the array is unsorted. If sorted, two pointers are another option.

**Pattern:** Hash Map / Complement Search  
Given an array and target, return indices of two different values whose sum equals target.

```text
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
```

### Easy – Brute force
Check every pair. Time `O(n²)`, extra space `O(1)`.

### Step-by-step dry run
1. Check i=0 (2) with j=1 (7): 2+7=9.
2. The target is found, so return indices [0,1].



**Java**
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++)
            for (int j = i + 1; j < nums.length; j++)
                if (nums[i] + nums[j] == target)
                    return new int[]{i, j};
        return new int[0];
    }
}
```
**Python**
```python
def two_sum(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []
```

### Best – Hash Map
For each value, calculate `need = target - value`. If need was seen, return both indices; otherwise store current value and index.

**Dry run:** 2 → need 7, store `2:0`; 7 → need 2, found at index 0; return `[0,1]`.

### Step-by-step dry run
1. Start with an empty map.
2. At index 0, value=2; need=9−2=7, not found, so store 2→0.
3. At index 1, value=7; need=2, found at index 0.
4. Return [0,1].



**Java**
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
**Python**
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
**Complexity:** Best `O(n)` average time, `O(n)` space. Test duplicates `[3,3]`, negative values, and no pair.

## 1.2 Move Zeroes

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Recognize stable in-place rearrangement

### 1. Problem statement
Move every zero in an integer array to the end while preserving the relative order of non-zero elements. Modify the array in place.

### 2. Input and output format
- **Input:** `nums = [0,1,0,3,12]`
- **Output:** `[1,3,12,0,0]`

### 3. Constraints
1 <= nums.length <= 10^4; values may be zero or non-zero.

### 4. Examples
- **Example:** `[0,0,1] → [1,0,0]`
- **Additional Example 2:** Input `nums=[0,0,0,2]` → Output `[2,0,0,0]`
- **Additional Example 3:** Input `nums=[1,2,3]` → Output `[1,2,3]`


### 5. Pattern identification — how to recognize the problem
Recognize stable in-place rearrangement. Start with an extra result array, then optimize with read/write pointers.

**Pattern:** Two Pointers / In-place  
Move all zeroes to the end, preserving non-zero order.

```text
Input: [0,1,0,3,12] → Output: [1,3,12,0,0]
```

### Easy – Extra array
### Step-by-step dry run
1. Start temp=[].
2. Read 0: skip.
3. Read 1: append → [1].
4. Read 0: skip.
5. Read 3 then 12: temp=[1,3,12].
6. Fill remaining two positions with zeroes → [1,3,12,0,0].



**Java**
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int[] temp = new int[nums.length];
        int k = 0;
        for (int x : nums) if (x != 0) temp[k++] = x;
        for (int i = 0; i < nums.length; i++) nums[i] = temp[i];
    }
}
```
**Python**
```python
def move_zeroes(nums):
    temp = [x for x in nums if x != 0]
    nums[:] = temp + [0] * (len(nums) - len(temp))
```
Time `O(n)`, extra space `O(n)`.

### Best – Read/write pointer
`write` is the next position for a non-zero. Scan and swap each non-zero into that position.

**Dry run:** `[0,1,0,3,12]` → move 1 to index 0 → move 3 to index 1 → move 12 to index 2 → `[1,3,12,0,0]`.

### Step-by-step dry run
1. Set write=0.
2. Read 0 at index 0: skip.
3. Read 1 at index 1: move it to index 0; write=1.
4. Skip 0 at index
2.
5. Move 3 to index 1; write=2.
6. Move 12 to index 2; write=3. Result=[1,3,12,0,0].



**Java**
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
**Python**
```python
def move_zeroes(nums):
    write = 0
    for read in range(len(nums)):
        if nums[read] != 0:
            nums[write], nums[read] = nums[read], nums[write]
            write += 1
```
Time `O(n)`, extra space `O(1)`. Edge cases: all zeroes, no zeroes, one item.

## 1.3 Maximum Subarray

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A contiguous range with maximum sum suggests enumerating subarrays first, then Kadane's algorithm: extend the current range or restart at the current element

### 1. Problem statement
Given an integer array, return the largest possible sum of a non-empty contiguous subarray.

### 2. Input and output format
- **Input:** `nums = [-2,1,-3,4,-1,2,1,-5,4]`
- **Output:** `6`

### 3. Constraints
1 <= nums.length <= 10^5; -10^4 <= nums[i] <= 10^4.

### 4. Examples
- **Example:** `[-2,1,-3,4,-1,2,1,-5,4] → 6; subarray [4,-1,2,1]`
- **Additional Example 2:** Input `nums=[1]` → Output `1`
- **Additional Example 3:** Input `nums=[-3,-2,-5]` → Output `-2`


### 5. Pattern identification — how to recognize the problem
A contiguous range with maximum sum suggests enumerating subarrays first, then Kadane's algorithm: extend the current range or restart at the current element.

**Pattern:** Kadane / Dynamic Programming  
Find the largest sum of a non-empty contiguous subarray.

```text
Input: [-2,1,-3,4,-1,2,1,-5,4]
Output: 6; best subarray = [4,-1,2,1]
```

### Easy – Enumerate all subarrays
### Step-by-step dry run
1. Start at index 0 and extend running sum: -2, -1, -4, 0, -1, 1, 2, -3,
1.
2. Repeat from index 1, then 2, and so on.
3. Starting at index 3 gives 4, 3, 5, 6, 1,
5.
4. The largest sum found across all starting positions is 6.



**Java**
```java
class Solution {
    public int maxSubArray(int[] a) {
        int best = a[0];
        for (int i = 0; i < a.length; i++) {
            int sum = 0;
            for (int j = i; j < a.length; j++) {
                sum += a[j];
                best = Math.max(best, sum);
            }
        }
        return best;
    }
}
```
**Python**
```python
def max_sub_array(a):
    best = a[0]
    for i in range(len(a)):
        total = 0
        for j in range(i, len(a)):
            total += a[j]
            best = max(best, total)
    return best
```
Time `O(n²)`, extra space `O(1)`.

### Best – Kadane
At each value, either start a new subarray or extend the current one:
`current = max(value, current + value)`, `best = max(best, current)`.

**Dry run:** start -2; then 1 → current 1; -3 → -2; 4 → 4; -1 → 3; 2 → 5; 1 → 6. Answer 6.

### Step-by-step dry run
1. Initialize current=best=-2.
2. Read 1: current=max(1,-1)=1; best=1.
3. Read -3: current=-2; best=1.
4. Read 4: current=4; best=4.
5. Read -1: current=3.
6. Read 2: current=5; read 1: current=6.
7. Read -5: current=1; read 4: current=5. Final best=6.



**Java**
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
**Python**
```python
def max_sub_array(nums):
    current = best = nums[0]
    for x in nums[1:]:
        current = max(x, current + x)
        best = max(best, current)
    return best
```
Time `O(n)`, space `O(1)`. Test all-negative and one-element arrays.

## 1.4 Missing Number

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A complete range with one missing value suggests sorting, the arithmetic-series sum, or XOR cancellation

### 1. Problem statement
An array contains n distinct numbers from the range 0 through n. Return the one number that is missing.

### 2. Input and output format
- **Input:** `nums = [3,0,1]`
- **Output:** `2`

### 3. Constraints
n == nums.length; all values are distinct and lie in [0,n].

### 4. Examples
- **Example:** `[0,1] → 2`
- **Additional Example 2:** Input `nums=[0,1]` → Output `2`
- **Additional Example 3:** Input `nums=[9,6,4,2,3,5,7,0,1]` → Output `8`


### 5. Pattern identification — how to recognize the problem
A complete range with one missing value suggests sorting, the arithmetic-series sum, or XOR cancellation.

**Pattern:** Math / XOR  
Array contains `n` distinct values from `0` through `n`; find the missing value.

```text
Input: [3,0,1] → Output: 2
```

### Easy – Sort and compare
### Step-by-step dry run
1. Sort [3,0,1] → [0,1,3].
2. Index 0 has 0; index 1 has
1.
3. At index 2, expected 2 but found
3.
4. Return 2.



**Java**
```java
import java.util.*;
class Solution {
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++)
            if (nums[i] != i) return i;
        return nums.length;
    }
}
```
**Python**
```python
def missing_number(nums):
    nums.sort()
    for i, x in enumerate(nums):
        if i != x:
            return i
    return len(nums)
```
Time `O(n log n)`.

### Best – Sum formula
Expected sum is `n*(n+1)/2`; subtract actual sum.

### Step-by-step dry run
1. n=3; expected sum=3×4/2=6.
2. Actual sum=3+0+1=4.
3. Missing=6−4=2.



**Java**
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
**Python**
```python
def missing_number(nums):
    n = len(nums)
    return n * (n + 1) // 2 - sum(nums)
```

### Alternative – XOR
Equal values cancel because `x ^ x = 0`.

### Step-by-step dry run
1. Start answer=3.
2. XOR index/value pairs: answer ^= 0 ^ 3, then ^= 1 ^ 0, then ^= 2 ^
1.
3. Matching values cancel in pairs.
4. The remaining value is 2.



**Java**
```java
class Solution {
    public int missingNumber(int[] nums) {
        int ans = nums.length;
        for (int i = 0; i < nums.length; i++) {
            ans ^= i;
            ans ^= nums[i];
        }
        return ans;
    }
}
```
**Python**
```python
def missing_number(nums):
    ans = len(nums)
    for i, x in enumerate(nums):
        ans ^= i
        ans ^= x
    return ans
```
Sum/XOR: `O(n)` time, `O(1)` extra space. Test missing 0 and missing n.

---

# 2. STRINGS

## 2.1 Reverse String

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** When values at opposite ends must exchange positions, use two pointers moving inward

### 1. Problem statement
Reverse the given character array in place.

### 2. Input and output format
- **Input:** `s = ['h','e','l','l','o']`
- **Output:** `['o','l','l','e','h']`

### 3. Constraints
1 <= s.length <= 10^5; modify the input array rather than returning a new one.

### 4. Examples
- **Example:** `['H','a','n','n','a','h'] → ['h','a','n','n','a','H']`
- **Additional Example 2:** Input `s=['H','a','n','n','a','h']` → Output `['h','a','n','n','a','H']`
- **Additional Example 3:** Input `s=['A']` → Output `['A']`


### 5. Pattern identification — how to recognize the problem
When values at opposite ends must exchange positions, use two pointers moving inward. An extra array is the simple baseline.

**Pattern:** Two Pointers. Reverse a character array in place.

### Easy – Extra copy
### Step-by-step dry run
1. Read the last character o and write to copy[0].
2. Read l, l, e, h into the next positions.
3. Copy [o,l,l,e,h] back to the original array.



**Java**
```java
class Solution {
    public void reverseString(char[] s) {
        char[] copy = new char[s.length];
        for (int i = 0; i < s.length; i++) copy[i] = s[s.length - 1 - i];
        for (int i = 0; i < s.length; i++) s[i] = copy[i];
    }
}
```
**Python**
```python
def reverse_string(s):
    s[:] = s[::-1]
```
Time `O(n)`, extra space `O(n)`.

### Best – Swap ends
### Step-by-step dry run
1. left=0,right=4: swap h and o → [o,e,l,l,h].
2. left=1,right=3: swap e and l → [o,l,l,e,h].
3. left=2,right=2: stop. Result=[o,l,l,e,h].



**Java**
```java
class Solution {
    public void reverseString(char[] s) {
        int l = 0, r = s.length - 1;
        while (l < r) {
            char t = s[l]; s[l] = s[r]; s[r] = t;
            l++; r--;
        }
    }
}
```
**Python**
```python
def reverse_string(s):
    l, r = 0, len(s) - 1
    while l < r:
        s[l], s[r] = s[r], s[l]
        l += 1
        r -= 1
```
Time `O(n)`, extra space `O(1)` for in-place version.

## 2.2 Valid Palindrome

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Palindrome checks compare mirrored characters

### 1. Problem statement
Return true if the string reads the same forward and backward after ignoring non-alphanumeric characters and letter case.

### 2. Input and output format
- **Input:** `s = 'A man, a plan, a canal: Panama'`
- **Output:** `true`

### 3. Constraints
1 <= s.length <= 2 * 10^5; compare letters/digits case-insensitively.

### 4. Examples
- **Example:** `'race a car' → false`
- **Additional Example 2:** Input `s='race a car'` → Output `false`
- **Additional Example 3:** Input `s=' '` → Output `true`


### 5. Pattern identification — how to recognize the problem
Palindrome checks compare mirrored characters. Clean the string first for the easy approach; skip irrelevant characters with two pointers for O(1) extra space.

**Pattern:** Two Pointers. Ignore non-alphanumeric characters and compare case-insensitively.

```text
Input: "A man, a plan, a canal: Panama" → true
```

### Easy – Clean and compare
### Step-by-step dry run
1. Keep only letters/digits and lowercase them: 'amanaplanacanalpanama'.
2. Reverse the cleaned string.
3. The reversed text is identical.
4. Return true.



**Java**
```java
class Solution {
    public boolean isPalindrome(String s) {
        StringBuilder b = new StringBuilder();
        for (char c : s.toCharArray())
            if (Character.isLetterOrDigit(c)) b.append(Character.toLowerCase(c));
        String x = b.toString();
        return x.equals(b.reverse().toString());
    }
}
```
**Python**
```python
def is_palindrome(s):
    x = ''.join(c.lower() for c in s if c.isalnum())
    return x == x[::-1]
```
Time `O(n)`, space `O(n)`.

### Best – Two pointers without new string
### Step-by-step dry run
1. Compare the first and last meaningful characters A/a: equal.
2. Skip spaces and punctuation.
3. Compare each next pair inward; every pair matches.
4. Pointers meet, so return true.



**Java**
```java
class Solution {
    public boolean isPalindrome(String s) {
        int l = 0, r = s.length() - 1;
        while (l < r) {
            while (l < r && !Character.isLetterOrDigit(s.charAt(l))) l++;
            while (l < r && !Character.isLetterOrDigit(s.charAt(r))) r--;
            if (Character.toLowerCase(s.charAt(l)) !=
                Character.toLowerCase(s.charAt(r))) return false;
            l++; r--;
        }
        return true;
    }
}
```
**Python**
```python
def is_palindrome(s):
    l, r = 0, len(s) - 1
    while l < r:
        while l < r and not s[l].isalnum(): l += 1
        while l < r and not s[r].isalnum(): r -= 1
        if s[l].lower() != s[r].lower(): return False
        l += 1
        r -= 1
    return True
```
Time `O(n)`, extra space `O(1)`. Test punctuation-only and mixed case.

## 2.3 Valid Anagram

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Anagram means equal character counts

### 1. Problem statement
Return true if strings s and t contain exactly the same characters with the same frequencies.

### 2. Input and output format
- **Input:** `s = 'anagram', t = 'nagaram'`
- **Output:** `true`

### 3. Constraints
s and t contain lowercase English letters in the fixed-array version; lengths can be up to 5 * 10^4.

### 4. Examples
- **Example:** `s = 'rat', t = 'car' → false`
- **Additional Example 2:** Input `s='rat', t='car'` → Output `false`
- **Additional Example 3:** Input `s='listen', t='silent'` → Output `true`


### 5. Pattern identification — how to recognize the problem
Anagram means equal character counts. Sort both strings for a simple solution; use a frequency array/map for linear time.

**Pattern:** Sorting / Frequency Count. Determine whether two strings contain the same characters with identical frequencies.

```text
Input: s = "anagram", t = "nagaram" → true
```

### Easy – Sort both
### Step-by-step dry run
1. Sort 'anagram' → 'aaagmnr'.
2. Sort 'nagaram' → 'aaagmnr'.
3. Compare sorted results; equal means true.



**Java**
```java
import java.util.*;
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) return false;
        char[] a = s.toCharArray(), b = t.toCharArray();
        Arrays.sort(a); Arrays.sort(b);
        return Arrays.equals(a, b);
    }
}
```
**Python**
```python
def is_anagram(s, t):
    return sorted(s) == sorted(t)
```
Time `O(n log n)`.

### Best – Frequency array (lowercase a-z)
### Step-by-step dry run
1. Start 26 counters at zero.
2. For every index, increment the character from s and decrement the character from t.
3. Since the strings have the same counts, every counter returns to zero.
4. Return true.



**Java**
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) return false;
        int[] count = new int[26];
        for (int i = 0; i < s.length(); i++) {
            count[s.charAt(i) - 'a']++;
            count[t.charAt(i) - 'a']--;
        }
        for (int x : count) if (x != 0) return false;
        return true;
    }
}
```
**Python**
```python
def is_anagram(s, t):
    if len(s) != len(t): return False
    count = [0] * 26
    for a, b in zip(s, t):
        count[ord(a)-97] += 1
        count[ord(b)-97] -= 1
    return all(x == 0 for x in count)
```
Time `O(n)`, space `O(1)` for fixed alphabet. For arbitrary Unicode, use a map.

## 2.4 First Non-Repeating Character

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** The word first means original string order matters

### 1. Problem statement
Find the first character in a string whose frequency is exactly one. Return the required sentinel if none exists.

### 2. Input and output format
- **Input:** `s = 'swiss'`
- **Output:** `'w'`

### 3. Constraints
1 <= s.length <= 10^5; clarify the expected return type/sentinel with the platform.

### 4. Examples
- **Example:** `'leetcode' → 'l'`
- **Additional Example 2:** Input `s='leetcode'` → Output `'l'`
- **Additional Example 3:** Input `s='aabb'` → Output `'$'`


### 5. Pattern identification — how to recognize the problem
The word first means original string order matters. Count frequencies, then scan the string again; nested counting is the brute-force baseline.

**Pattern:** Frequency Count + Original Order. Return the first character with frequency one; use the platform’s required sentinel (`'$'`, `-1`, or index).

```text
Input: "swiss" → 'w'
```

### Easy – Nested count
### Step-by-step dry run
1. Examine 's': count it through the full string; frequency is 3, so skip.
2. Examine 'w': frequency is
1.
3. Return 'w' immediately.



**Java**
```java
class Solution {
    public char firstUnique(String s) {
        for (int i = 0; i < s.length(); i++) {
            int count = 0;
            for (int j = 0; j < s.length(); j++)
                if (s.charAt(i) == s.charAt(j)) count++;
            if (count == 1) return s.charAt(i);
        }
        return '$';
    }
}
```
**Python**
```python
def first_unique(s):
    for c in s:
        if s.count(c) == 1:
            return c
    return '$'
```
Time `O(n²)`.

### Best – Two passes with frequency map
### Step-by-step dry run
1. First pass builds counts, including s=3 and w=1.
2. Second pass preserves original order: s is skipped because count=3.
3. w has count=1; return 'w'.



**Java**
```java
import java.util.*;
class Solution {
    public char firstUnique(String s) {
        Map<Character, Integer> freq = new HashMap<>();
        for (char c : s.toCharArray())
            freq.put(c, freq.getOrDefault(c, 0) + 1);
        for (char c : s.toCharArray())
            if (freq.get(c) == 1) return c;
        return '$';
    }
}
```
**Python**
```python
from collections import Counter
def first_unique(s):
    freq = Counter(s)
    for c in s:
        if freq[c] == 1:
            return c
    return '$'
```
Time `O(n)`, space `O(k)` distinct characters.

---

# 3. HASHING

## 3.1 Two Sum

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** For each value, look for its complement (target - value)

### 1. Problem statement
Given an integer array nums and an integer target, return the indices of two distinct elements whose values add up to target.

### 2. Input and output format
- **Input:** `nums = [2,7,11,15], target = 9`
- **Output:** `[0,1]`

### 3. Constraints
2 <= nums.length <= 10^4; values and target may be negative; exactly one solution is commonly guaranteed by the interview version.

### 4. Examples
- **Example:** `nums = [3,2,4], target = 6 → [1,2]`
- **Additional Example 2:** Input `nums=[2,5,5,11], target=10` → Output `[1,2]`
- **Additional Example 3:** Input `nums=[3,3], target=6` → Output `[0,1]`


### 5. Pattern identification — how to recognize the problem
For each value, look for its complement (target - value). Use brute-force pair checking first; use a HashMap when the array is unsorted. If sorted, two pointers are another option.

This is the same Two Sum problem, viewed through the hashing pattern. The brute-force approach checks every pair. The optimal Hash Map approach stores `value → index` and searches for `target - current`. Check for the complement before inserting the current value so the same element is never used twice.

### Easy – Brute force

### Step-by-step dry run
1. Use `nums=[2,7,11,15]`, `target=9`.
2. Compare index 0 and 1: `2+7=9`.
3. The target is found; return `[0,1]`.



**Java**
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[0];
    }
}
```

**Python**
```python
def two_sum(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []
```

### Best – Hash Map

### Step-by-step dry run
1. Start with an empty map.
2. At index 0, value 2 needs 7; it is absent, so store `2 → 0`.
3. At index 1, value 7 needs 2; 2 is present at index 0.
4. Return `[0,1]`.



**Java**
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

**Python**
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

**Complexity:** Brute force `O(n²)` time and `O(1)` extra space. Hash Map `O(n)` average time and `O(n)` extra space.

## 3.2 Frequency Count

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Repeated counting suggests a HashMap

### 1. Problem statement
Count how many times each distinct value appears in an array.

### 2. Input and output format
- **Input:** `a = [1,2,2,3,1,2]`
- **Output:** `{1:2, 2:3, 3:1}`

### 3. Constraints
0 <= a.length <= 10^5; values may be negative; output ordering depends on the required format.

### 4. Examples
- **Example:** `[4,4,4] → {4:3}`
- **Additional Example 2:** Input `a=[4,4,4]` → Output `{4:3}`
- **Additional Example 3:** Input `a=[-1,2,-1]` → Output `{-1:2, 2:1}`


### 5. Pattern identification — how to recognize the problem
Repeated counting suggests a HashMap. Nested scans are easy to explain; a map counts all values in one pass.

**Pattern:** Hash Map. Count occurrences of each number.

```text
Input: [1,2,2,3,1,2] → 1:2, 2:3, 3:1
```

### Easy – Nested loops
### Step-by-step dry run
1. Start with first unvisited value 1; scan the remaining array and find two occurrences.
2. Mark those two positions visited.
3. Next unvisited value 2 occurs three times; mark all three.
4. Value 3 occurs once. Output: 1→2, 2→3, 3→1.



**Java**
```java
class Solution {
    public void frequency(int[] a) {
        boolean[] visited = new boolean[a.length];
        for (int i = 0; i < a.length; i++) {
            if (visited[i]) continue;
            int count = 0;
            for (int j = i; j < a.length; j++) {
                if (a[i] == a[j]) { count++; visited[j] = true; }
            }
            System.out.println(a[i] + " -> " + count);
        }
    }
}
```
**Python**
```python
def frequency(a):
    visited = [False] * len(a)
    for i in range(len(a)):
        if visited[i]: continue
        count = 0
        for j in range(i, len(a)):
            if a[i] == a[j]:
                count += 1
                visited[j] = True
        print(a[i], "->", count)
```
Time `O(n²)`.

### Best – Hash Map
### Step-by-step dry run
1. Empty map.
2. Read 1 → {1:1}.
3. Read 2 twice → {1:1,2:2}.
4. Read 3 → {1:1,2:2,3:1}.
5. Read 1 → {1:2,2:2,3:1}.
6. Read 2 → {1:2,2:3,3:1}.



**Java**
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
**Python**
```python
from collections import Counter
def frequency(a):
    return Counter(a)
```
Time `O(n)` average, space `O(k)`. `LinkedHashMap` preserves insertion order.

## 3.3 Duplicate Elements

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A duplicate is a repeated value

### 1. Problem statement
Determine whether an array contains at least one value that appears more than once.

### 2. Input and output format
- **Input:** `a = [1,2,3,1]`
- **Output:** `true`

### 3. Constraints
0 <= a.length <= 10^5; values may be negative.

### 4. Examples
- **Example:** `[1,2,3] → false`
- **Additional Example 2:** Input `a=[1,2,3]` → Output `false`
- **Additional Example 3:** Input `a=[0,0]` → Output `true`


### 5. Pattern identification — how to recognize the problem
A duplicate is a repeated value. Compare all pairs first; use a HashSet to detect a value already seen.

**Pattern:** Hash Set. Return true if any value occurs more than once.

### Easy – Compare pairs
### Step-by-step dry run
1. Compare first 1 with 2: different.
2. Compare 1 with 3: different.
3. Compare 1 with later 1: equal.
4. Return true.



**Java**
```java
class Solution {
    public boolean containsDuplicate(int[] a) {
        for (int i = 0; i < a.length; i++)
            for (int j = i + 1; j < a.length; j++)
                if (a[i] == a[j]) return true;
        return false;
    }
}
```
**Python**
```python
def contains_duplicate(a):
    for i in range(len(a)):
        for j in range(i + 1, len(a)):
            if a[i] == a[j]: return True
    return False
```
Time `O(n²)`, space `O(1)`.

### Best – Hash Set
### Step-by-step dry run
1. Start with an empty set.
2. Add 1, then 2, then
3.
3. Read the last 1; it is already in the set.
4. Return true.



**Java**
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
**Python**
```python
def contains_duplicate(a):
    seen = set()
    for x in a:
        if x in seen: return True
        seen.add(x)
    return False
```
Time `O(n)` average, space `O(n)`. Alternative: sort then compare neighbors (`O(n log n)`).

---

# 4. SEARCHING

## 4.1 Binary Search

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** The sorted order allows eliminating half the search range after each comparison

### 1. Problem statement
Given a sorted array and a target, return its index or -1 if it is absent.

### 2. Input and output format
- **Input:** `a = [-1,0,3,5,9,12], target = 9`
- **Output:** `4`

### 3. Constraints
Array is sorted in ascending order; 0 <= length <= 10^4.

### 4. Examples
- **Example:** `a = [1,3,5], target = 2 → -1`
- **Additional Example 2:** Input `a=[1,3,5], target=2` → Output `-1`
- **Additional Example 3:** Input `a=[2], target=2` → Output `0`


### 5. Pattern identification — how to recognize the problem
The sorted order allows eliminating half the search range after each comparison. Linear search is the baseline; binary search is optimal.

**Pattern:** Divide and Conquer. Input must be sorted. Return target index or `-1`.

```text
Input: [-1,0,3,5,9,12], target = 9 → 4
```

### Easy – Linear Search
### Step-by-step dry run
1. Check index 0: -1 is not
9.
2. Index 1: 0 is not
9.
3. Index 2: 3 is not
9.
4. Index 3: 5 is not
9.
5. Index 4 contains 9; return 4.



**Java**
```java
class Solution {
    public int search(int[] a, int target) {
        for (int i = 0; i < a.length; i++)
            if (a[i] == target) return i;
        return -1;
    }
}
```
**Python**
```python
def search(a, target):
    for i, x in enumerate(a):
        if x == target: return i
    return -1
```
Time `O(n)`.

### Best – Iterative Binary Search
Compare middle. If middle is smaller, move left boundary right; otherwise move right boundary left.

**Dry run:** `[1,3,5,7,9]`, target 7: mid 5 → search right; mid 7 → found.

### Step-by-step dry run
1. left=0,right=5,mid=2; value 3<9, so left=3.
2. mid=4; value 9 matches.
3. Return index 4.



**Java**
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
**Python**
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
Time `O(log n)`, space `O(1)`.

### Alternative – Recursive Binary Search
### Step-by-step dry run
1. Call search(0,5): mid=2, value 3<9; recurse into (3,5).
2. Mid=4, value 9 matches.
3. Return 4 up the call stack.



**Java**
```java
class Solution {
    public int search(int[] a, int target) {
        return solve(a, target, 0, a.length - 1);
    }
    private int solve(int[] a, int target, int l, int r) {
        if (l > r) return -1;
        int m = l + (r - l) / 2;
        if (a[m] == target) return m;
        if (a[m] < target) return solve(a, target, m + 1, r);
        return solve(a, target, l, m - 1);
    }
}
```
**Python**
```python
def search(a, target):
    def solve(l, r):
        if l > r: return -1
        m = l + (r - l) // 2
        if a[m] == target: return m
        if a[m] < target: return solve(m + 1, r)
        return solve(l, m - 1)
    return solve(0, len(a) - 1)
```
Time `O(log n)`, recursive stack `O(log n)`.

## 4.2 First and Last Position

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Duplicates in sorted data call for boundary binary searches: one finds the leftmost match and one the rightmost

### 1. Problem statement
In a sorted array, return the first and last index of a target value, or [-1,-1] if it is absent.

### 2. Input and output format
- **Input:** `a = [5,7,7,8,8,10], target = 8`
- **Output:** `[3,4]`

### 3. Constraints
0 <= length <= 10^5; array is sorted in non-decreasing order.

### 4. Examples
- **Example:** `a = [1,2,2,2,3], target = 2 → [1,3]`
- **Additional Example 2:** Input `a=[1,2,2,2,3], target=2` → Output `[1,3]`
- **Additional Example 3:** Input `a=[1,2,3], target=4` → Output `[-1,-1]`


### 5. Pattern identification — how to recognize the problem
Duplicates in sorted data call for boundary binary searches: one finds the leftmost match and one the rightmost.

**Pattern:** Boundary Binary Search. In a sorted array with duplicates, return first and last target indices.

```text
Input: [5,7,7,8,8,10], target = 8 → [3,4]
```

### Easy – Linear scan
### Step-by-step dry run
1. Visit indices 0 through
5.
2. At index 3, first=3 and last=3.
3. At index 4, update last=4.
4. Return [3,4].



**Java**
```java
class Solution {
    public int[] searchRange(int[] a, int target) {
        int first = -1, last = -1;
        for (int i = 0; i < a.length; i++) {
            if (a[i] == target) {
                if (first == -1) first = i;
                last = i;
            }
        }
        return new int[]{first, last};
    }
}
```
**Python**
```python
def search_range(a, target):
    first = last = -1
    for i, x in enumerate(a):
        if x == target:
            if first == -1: first = i
            last = i
    return [first, last]
```
Time `O(n)`.

### Best – Two binary searches
One search continues left after finding target; the other continues right.

### Step-by-step dry run
First search: mid=2 gives 7<8, move right; mid=4 gives 8, save 4 and continue left; mid=3 gives 8, save 3 and continue left. First=3. Last search moves right after a match and finishes at index
4. Return [3,4].



**Java**
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
**Python**
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
Time `O(log n)`, space `O(1)`. Test absent target and all-equal array.

---

# 5. SORTING

## 5.1 Bubble Sort

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Adjacent comparisons and swaps identify Bubble Sort

### 1. Problem statement
Sort an integer array in ascending order by repeatedly swapping adjacent out-of-order elements.

### 2. Input and output format
- **Input:** `a = [5,1,4,2,8]`
- **Output:** `[1,2,4,5,8]`

### 3. Constraints
Array length may be 0 or greater; values may be negative or repeated.

### 4. Examples
- **Example:** `[3,2,1] → [1,2,3]`
- **Additional Example 2:** Input `a=[3,2,1]` → Output `[1,2,3]`
- **Additional Example 3:** Input `a=[1,2,3]` → Output `[1,2,3]`


### 5. Pattern identification — how to recognize the problem
Adjacent comparisons and swaps identify Bubble Sort. Use an early-stop flag to avoid unnecessary passes.

**Idea:** Compare adjacent values and swap if out of order. Largest unsorted value moves to the end each pass.

### Step-by-step dry run
Start [5,1,4,2,8].
Pass 1: swap 5/1 → [1,5,4,2,8]; swap 5/4 → [1,4,5,2,8]; swap 5/2 → [1,4,2,5,8]; 5/8 stays.
Pass 2: 1/4 stays; swap 4/2 → [1,2,4,5,8]; remaining pairs stay. Pass 3 has no swaps, so stop. Final [1,2,4,5,8].



**Java**
```java
class SortDemo {
    static void bubbleSort(int[] a) {
        for (int i = 0; i < a.length - 1; i++) {
            boolean swapped = false;
            for (int j = 0; j < a.length - 1 - i; j++) {
                if (a[j] > a[j + 1]) {
                    int t = a[j]; a[j] = a[j + 1]; a[j + 1] = t;
                    swapped = true;
                }
            }
            if (!swapped) break;
        }
    }
}
```
**Python**
```python
def bubble_sort(a):
    for i in range(len(a) - 1):
        swapped = False
        for j in range(len(a) - 1 - i):
            if a[j] > a[j + 1]:
                a[j], a[j + 1] = a[j + 1], a[j]
                swapped = True
        if not swapped: break
```
Best `O(n)` with early stop; average/worst `O(n²)`. Space `O(1)`. Stable.

## 5.2 Selection Sort

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** If each pass selects the smallest remaining item and places it at the front, it is Selection Sort

### 1. Problem statement
Sort an integer array by repeatedly selecting the minimum value from the unsorted suffix.

### 2. Input and output format
- **Input:** `a = [64,25,12,22,11]`
- **Output:** `[11,12,22,25,64]`

### 3. Constraints
Array length may be 0 or greater; values may repeat.

### 4. Examples
- **Example:** `[3,1,2] → [1,2,3]`
- **Additional Example 2:** Input `a=[3,1,2]` → Output `[1,2,3]`
- **Additional Example 3:** Input `a=[5,5,2]` → Output `[2,5,5]`


### 5. Pattern identification — how to recognize the problem
If each pass selects the smallest remaining item and places it at the front, it is Selection Sort.

**Idea:** Find minimum in unsorted suffix and swap into current position.

### Step-by-step dry run
Start [64,25,12,22,11]. Pass 1 minimum is 11; swap with 64 → [11,25,12,22,64]. Pass 2 minimum of suffix is 12; swap with 25 → [11,12,25,22,64]. Pass 3 minimum is 22; swap → [11,12,22,25,64]. Pass 4 is already ordered. Final [11,12,22,25,64].



**Java**
```java
class SortDemo {
    static void selectionSort(int[] a) {
        for (int i = 0; i < a.length - 1; i++) {
            int min = i;
            for (int j = i + 1; j < a.length; j++)
                if (a[j] < a[min]) min = j;
            int t = a[i]; a[i] = a[min]; a[min] = t;
        }
    }
}
```
**Python**
```python
def selection_sort(a):
    for i in range(len(a) - 1):
        m = i
        for j in range(i + 1, len(a)):
            if a[j] < a[m]: m = j
        a[i], a[m] = a[m], a[i]
```
Best/average/worst `O(n²)`, space `O(1)`. Standard swap version is not stable.

## 5.3 Insertion Sort

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A growing sorted prefix and shifting larger elements indicate Insertion Sort

### 1. Problem statement
Sort an array by inserting each next element into its correct position in the already-sorted prefix.

### 2. Input and output format
- **Input:** `a = [5,2,4,6,1,3]`
- **Output:** `[1,2,3,4,5,6]`

### 3. Constraints
Array length may be 0 or greater; works especially well for small or nearly sorted inputs.

### 4. Examples
- **Example:** `[3,1,2] → [1,2,3]`
- **Additional Example 2:** Input `a=[3,1,2]` → Output `[1,2,3]`
- **Additional Example 3:** Input `a=[1,2,3]` → Output `[1,2,3]`


### 5. Pattern identification — how to recognize the problem
A growing sorted prefix and shifting larger elements indicate Insertion Sort.

**Idea:** Grow a sorted prefix; shift larger values right and insert the current key.

### Step-by-step dry run
Start [5,2,4,6,1,3]. Insert 2: shift 5 → [2,5,4,6,1,3]. Insert 4: shift 5 → [2,4,5,6,1,3]. Insert 6: no shift. Insert 1: shift 6,5,4,2 → [1,2,4,5,6,3]. Insert 3: shift 6,5,4 → [1,2,3,4,5,6].



**Java**
```java
class SortDemo {
    static void insertionSort(int[] a) {
        for (int i = 1; i < a.length; i++) {
            int key = a[i], j = i - 1;
            while (j >= 0 && a[j] > key) {
                a[j + 1] = a[j];
                j--;
            }
            a[j + 1] = key;
        }
    }
}
```
**Python**
```python
def insertion_sort(a):
    for i in range(1, len(a)):
        key, j = a[i], i - 1
        while j >= 0 and a[j] > key:
            a[j + 1] = a[j]
            j -= 1
        a[j + 1] = key
```
Best `O(n)` on sorted input; average/worst `O(n²)`, space `O(1)`. Stable; good for small/nearly sorted arrays.

## 5.4 Merge Sort

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Repeatedly split into halves and merge sorted pieces: Merge Sort

### 1. Problem statement
Sort an array using divide and conquer: recursively sort halves, then merge the sorted halves.

### 2. Input and output format
- **Input:** `a = [38,27,43,3,9,82,10]`
- **Output:** `[3,9,10,27,38,43,82]`

### 3. Constraints
Array length may be 0 or greater; preserve duplicates.

### 4. Examples
- **Example:** `[4,1,3,2] → [1,2,3,4]`
- **Additional Example 2:** Input `a=[4,1,3,2]` → Output `[1,2,3,4]`
- **Additional Example 3:** Input `a=[5,-1,5,0]` → Output `[-1,0,5,5]`


### 5. Pattern identification — how to recognize the problem
Repeatedly split into halves and merge sorted pieces: Merge Sort. It guarantees O(n log n).

**Idea:** Divide into halves, sort each half recursively, merge sorted halves.

### Step-by-step dry run
Start [38,27,43,3]. Split into [38,27] and [43,3], then into single elements [38],[27],[43],[3]. Merge first pair → [27,38]. Merge second pair → [3,43]. Final merge compares 27 with 3 (take 3), then 27 with 43 (take 27), then 38, then 43. Result [3,27,38,43].



**Java**
```java
class SortDemo {
    static void mergeSort(int[] a, int l, int r) {
        if (l >= r) return;
        int m = l + (r - l) / 2;
        mergeSort(a, l, m);
        mergeSort(a, m + 1, r);
        int[] temp = new int[r - l + 1];
        int i = l, j = m + 1, k = 0;
        while (i <= m && j <= r)
            temp[k++] = (a[i] <= a[j]) ? a[i++] : a[j++];
        while (i <= m) temp[k++] = a[i++];
        while (j <= r) temp[k++] = a[j++];
        for (int x = 0; x < temp.length; x++) a[l + x] = temp[x];
    }
}
```
**Python**
```python
def merge_sort(a):
    if len(a) <= 1: return a
    m = len(a) // 2
    left, right = merge_sort(a[:m]), merge_sort(a[m:])
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    return result + left[i:] + right[j:]
```
Time `O(n log n)` in all cases; extra space `O(n)`; stable. Java sorts the given array; Python returns a new list.

## 5.5 Quick Sort

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A pivot and partition operation identify Quick Sort

### 1. Problem statement
Sort an array by selecting a pivot, partitioning values around it, and recursively sorting the partitions.

### 2. Input and output format
- **Input:** `a = [10,7,8,9,1,5]`
- **Output:** `[1,5,7,8,9,10]`

### 3. Constraints
Array length may be 0 or greater; pivot strategy affects performance.

### 4. Examples
- **Example:** `[3,7,2,5] → [2,3,5,7]`
- **Additional Example 2:** Input `a=[3,7,2,5]` → Output `[2,3,5,7]`
- **Additional Example 3:** Input `a=[4,4,1]` → Output `[1,4,4]`


### 5. Pattern identification — how to recognize the problem
A pivot and partition operation identify Quick Sort. Average O(n log n), worst O(n²).

**Idea:** Choose pivot, partition smaller/equal values to its left and larger values to its right, then recursively sort partitions.

### Step-by-step dry run — Java in-place partition
1. Start with `[10,7,8,9,1,5]`; choose the last value `5` as pivot.
2. Scan 10, 7, 8, and 9: each is greater than 5, so none moves to the left partition.
3. Scan 1: it is smaller than 5, so swap it into the first position → `[1,7,8,9,10,5]`.
4. Place the pivot immediately after the smaller values by swapping 7 and 5 → `[1,5,8,9,10,7]`. Pivot 5 is now in its final position.
5. Recursively sort the right partition `[8,9,10,7]`, then its smaller partitions, until each contains at most one element.
6. Final sorted result: `[1,5,7,8,9,10]`.



### Step-by-step dry run — Python list-partition version
1. Choose pivot 5 (last value).
2. From the remaining values `[10,7,8,9,1]`, collect values `<= 5` into `[1]`.
3. Collect values `> 5` into `[10,7,8,9]`.
4. Recursively sort the left list (still `[1]`) and right list (`[7,8,9,10]`).
5. Combine left + pivot + right → `[1,5,7,8,9,10]`.



**Java – In-place Lomuto partition**
```java
class SortDemo {
    static void quickSort(int[] a, int low, int high) {
        if (low >= high) return;
        int p = partition(a, low, high);
        quickSort(a, low, p - 1);
        quickSort(a, p + 1, high);
    }
    static int partition(int[] a, int low, int high) {
        int pivot = a[high], i = low - 1;
        for (int j = low; j < high; j++) {
            if (a[j] <= pivot) {
                i++;
                int t = a[i]; a[i] = a[j]; a[j] = t;
            }
        }
        int t = a[i + 1]; a[i + 1] = a[high]; a[high] = t;
        return i + 1;
    }
}
```
**Python – readable recursive version**
```python
def quick_sort(a):
    if len(a) <= 1: return a
    pivot = a[-1]
    left = [x for x in a[:-1] if x <= pivot]
    right = [x for x in a[:-1] if x > pivot]
    return quick_sort(left) + [pivot] + quick_sort(right)
```
Average/best `O(n log n)`, worst `O(n²)`; in-place Java version uses recursion stack (`O(log n)` average, `O(n)` worst). Python version creates extra lists. Standard quick sort is not stable. Randomized pivot reduces poor-pivot risk.

### Sorting comparison

| Algorithm | Best | Average | Worst | Extra space | Stable? |
|---|---:|---:|---:|---:|---|
| Bubble (optimized) | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | Usually no |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) avg stack | No |

---

# 6. TWO POINTERS

## 6.1 Pair Sum in Sorted Array

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Sorted input plus pair-sum suggests two pointers: increase the left pointer for a small sum, decrease right for a large sum

### 1. Problem statement
Given a sorted array and target, find two different indices whose values sum to the target.

### 2. Input and output format
- **Input:** `a = [1,2,4,6,10], target = 8`
- **Output:** `[1,3]`

### 3. Constraints
Array is sorted; indices must be different; return convention depends on platform.

### 4. Examples
- **Example:** `[2,3,5,8], target = 10 → [0,3]`
- **Additional Example 2:** Input `a=[2,3,5,8], target=10` → Output `[0,3]`
- **Additional Example 3:** Input `a=[1,2,4], target=20` → Output `[-1,-1]`


### 5. Pattern identification — how to recognize the problem
Sorted input plus pair-sum suggests two pointers: increase the left pointer for a small sum, decrease right for a large sum.

**Pattern:** Two Pointers. Find two values summing to target.

```text
Input: [1,2,4,6,10], target = 8 → indices [1,3]
```

### Easy – Brute force
### Step-by-step dry run
1. Compare indices 0,1: 1+2=3.
2. Compare 0,2: 1+4=5.
3. Compare 0,3: 1+6=7.
4. Compare 1,3: 2+6=8.
5. Return [1,3].



**Java**
```java
class Solution {
    public int[] pairSum(int[] a, int target) {
        for (int i = 0; i < a.length; i++)
            for (int j = i + 1; j < a.length; j++)
                if (a[i] + a[j] == target) return new int[]{i, j};
        return new int[]{-1, -1};
    }
}
```
**Python**
```python
def pair_sum(a, target):
    for i in range(len(a)):
        for j in range(i + 1, len(a)):
            if a[i] + a[j] == target: return [i, j]
    return [-1, -1]
```
Time `O(n²)`.

### Best – Two pointers (sorted input)
If sum is too small, move left forward; if too large, move right backward.

### Step-by-step dry run
1. left=0 (1), right=4 (10), sum=11>8; move right to
3.
2. 1+6=7<8; move left to
1.
3. 2+6=8; return [1,3].



**Java**
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
**Python**
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
Time `O(n)`, space `O(1)`. Array must be sorted; for unsorted input and original indices, use Two Sum hash map.

## 6.2 Remove Duplicates from Sorted Array

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Sorted duplicates are adjacent

### 1. Problem statement
Remove duplicates in place from a sorted array so each value appears once; return the number of unique values.

### 2. Input and output format
- **Input:** `a = [0,0,1,1,1,2,2,3,3,4]`
- **Output:** `5; first five values become [0,1,2,3,4]`

### 3. Constraints
Array is sorted; modify in place; returned k identifies the valid prefix.

### 4. Examples
- **Example:** `[1,1,2] → k=2; prefix [1,2]`
- **Additional Example 2:** Input `a=[1,1,2]` → Output `k=2; prefix=[1,2]`
- **Additional Example 3:** Input `a=[1,2,3]` → Output `k=3; prefix=[1,2,3]`


### 5. Pattern identification — how to recognize the problem
Sorted duplicates are adjacent. Keep a write pointer for the next unique value.

**Pattern:** Slow/Fast Pointers. Keep one copy of each value; return unique count `k`, with unique values in first `k` positions.

### Easy – Set/list
### Step-by-step dry run
1. Add first 1 to ordered set.
2. Ignore the second
1.
3. Add 2 and ignore its duplicate.
4. Add
3.
5. Copy [1,2,3] to the front and return k=3.



**Java**
```java
import java.util.*;
class Solution {
    public int removeDuplicates(int[] a) {
        Set<Integer> set = new LinkedHashSet<>();
        for (int x : a) set.add(x);
        int i = 0;
        for (int x : set) a[i++] = x;
        return i;
    }
}
```
**Python**
```python
def remove_duplicates(a):
    unique = list(dict.fromkeys(a))
    a[:len(unique)] = unique
    return len(unique)
```
Time `O(n)` average, extra space `O(n)`.

### Best – In-place write pointer
### Step-by-step dry run
1. Start write=1.
2. Read second 1; equals last written value, skip.
3. Read 2; copy to index 1, write=2.
4. Read next 2; skip.
5. Read 3; copy to index 2, write=3. Return k=3.



**Java**
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
**Python**
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
Time `O(n)`, space `O(1)`. Input must be sorted.

---

# 7. SLIDING WINDOW

## 7.1 Maximum Sum Subarray of Size K

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A fixed-length contiguous range suggests a fixed sliding window: subtract the outgoing value and add the incoming value

### 1. Problem statement
Find the maximum sum among all contiguous subarrays containing exactly k elements.

### 2. Input and output format
- **Input:** `a = [2,1,5,1,3,2], k = 3`
- **Output:** `9`

### 3. Constraints
1 <= k <= a.length; values may be negative.

### 4. Examples
- **Example:** `[1,4,2,10,2,3], k=2 → 12`
- **Additional Example 2:** Input `a=[1,4,2,10,2,3], k=2` → Output `12`
- **Additional Example 3:** Input `a=[-5,-2,-3], k=2` → Output `-5`


### 5. Pattern identification — how to recognize the problem
A fixed-length contiguous range suggests a fixed sliding window: subtract the outgoing value and add the incoming value.

**Pattern:** Fixed-Size Sliding Window. Find maximum sum of exactly `k` consecutive elements.

```text
Input: [2,1,5,1,3,2], k=3 → 9 ([5,1,3])
```

### Easy – Recalculate each window
### Step-by-step dry run
1. Window [2,1,5] sums to
8.
2. Window [1,5,1] sums to
7.
3. Window [5,1,3] sums to
9.
4. Window [1,3,2] sums to
6.
5. Maximum is 9.



**Java**
```java
class Solution {
    public int maxSum(int[] a, int k) {
        int best = Integer.MIN_VALUE;
        for (int i = 0; i <= a.length - k; i++) {
            int sum = 0;
            for (int j = i; j < i + k; j++) sum += a[j];
            best = Math.max(best, sum);
        }
        return best;
    }
}
```
**Python**
```python
def max_sum(a, k):
    best = float('-inf')
    for i in range(len(a) - k + 1):
        best = max(best, sum(a[i:i+k]))
    return best
```
Time `O(nk)`.

### Best – Sliding Window
Compute first `k` sum. Each slide subtracts outgoing value and adds incoming value.

**Dry run:** first window 2+1+5=8; slide: 8-2+1=7; slide: 7-1+3=9; slide: 9-5+2=6. Best 9.

### Step-by-step dry run
1. First window sum=8.
2. Slide: subtract outgoing 2 and add incoming 1 →
7.
3. Slide: subtract 1 and add 3 →
9.
4. Slide: subtract 5 and add 2 →
6.
5. Best remains 9.



**Java**
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
**Python**
```python
def max_sum(a, k):
    window = sum(a[:k])
    best = window
    for right in range(k, len(a)):
        window += a[right] - a[right-k]
        best = max(best, window)
    return best
```
Time `O(n)`, space `O(1)`. Assumption `1 <= k <= n`.

## 7.2 Longest Substring Without Repeating Characters

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A longest contiguous substring with a uniqueness condition suggests a variable sliding window and a set/map of last-seen positions

### 1. Problem statement
Return the length of the longest substring containing no repeated characters.

### 2. Input and output format
- **Input:** `s = 'abcabcbb'`
- **Output:** `3`

### 3. Constraints
0 <= s.length <= 5 * 10^4; characters may include letters, digits, symbols, and spaces.

### 4. Examples
- **Example:** `'bbbbb' → 1; 'pwwkew' → 3`
- **Additional Example 2:** Input `s='bbbbb'` → Output `1`
- **Additional Example 3:** Input `s='pwwkew'` → Output `3`


### 5. Pattern identification — how to recognize the problem
A longest contiguous substring with a uniqueness condition suggests a variable sliding window and a set/map of last-seen positions.

**Pattern:** Variable Sliding Window + Hash Map. Find length of longest substring with all distinct characters.

```text
Input: "abcabcbb" → 3 ("abc")
```

### Easy – Expand from each start
### Step-by-step dry run
1. Start at index 0: add a,b,c; next a repeats, so length=3.
2. Start at index 1: b,c,a are unique before b repeats; length=3.
3. Remaining starts produce no longer substring.
4. Return 3.



**Java**
```java
import java.util.*;
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int best = 0;
        for (int i = 0; i < s.length(); i++) {
            Set<Character> seen = new HashSet<>();
            for (int j = i; j < s.length(); j++) {
                if (seen.contains(s.charAt(j))) break;
                seen.add(s.charAt(j));
                best = Math.max(best, j - i + 1);
            }
        }
        return best;
    }
}
```
**Python**
```python
def longest_unique(s):
    best = 0
    for i in range(len(s)):
        seen = set()
        for j in range(i, len(s)):
            if s[j] in seen: break
            seen.add(s[j])
            best = max(best, j-i+1)
    return best
```
Time `O(n²)` expected.

### Best – Sliding window with last-seen index
Move left beyond the previous occurrence, but never move left backward.

### Step-by-step dry run
1. Read a,b,c: window [0,2], best=3.
2. Read a at index 3; previous a was 0, so left=1.
3. Read b at 4; previous b=1, so left=2.
4. Read c at 5; previous c=2, so left=3.
5. Read b at 6; previous b=4, so left=5.
6. Read b at 7; left=7. Best remains 3.



**Java**
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
**Python**
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
Time `O(n)` average, space `O(k)`. Test empty, all same, all unique, and repeated chars outside current window.

---

# 8. LINKED LISTS

Use this node definition in Java examples:
```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}
```
Python:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

## 8.1 Reverse Linked List

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Linked-list reversal requires changing links, not just values

### 1. Problem statement
Reverse the next pointers of a singly linked list and return the new head.

### 2. Input and output format
- **Input:** `head = 1 → 2 → 3 → null`
- **Output:** `3 → 2 → 1 → null`

### 3. Constraints
0 <= number of nodes <= 5000; node values may be negative.

### 4. Examples
- **Example:** `head = 1 → 2 → null → 2 → 1 → null`
- **Additional Example 2:** Input `head=1→2→null` → Output `2→1→null`
- **Additional Example 3:** Input `head=null` → Output `null`


### 5. Pattern identification — how to recognize the problem
Linked-list reversal requires changing links, not just values. Use a stack as a simple baseline; use prev/current/next pointers for O(1) space.

**Pattern:** Pointer Reversal. `1 → 2 → 3 → null` becomes `3 → 2 → 1 → null`.

### Easy – Store nodes in stack/list
### Step-by-step dry run
1. Traverse 1→2→3 and store references in that order.
2. Pop 3 and make it the new head.
3. Link 3→2.
4. Link 2→1.
5. Set 1.next=null. Result 3→2→1.



**Java**
```java
import java.util.*;
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) return null;
        Stack<ListNode> st = new Stack<>();
        for (ListNode cur = head; cur != null; cur = cur.next) st.push(cur);
        ListNode newHead = st.pop(), cur = newHead;
        while (!st.isEmpty()) {
            cur.next = st.pop();
            cur = cur.next;
        }
        cur.next = null;
        return newHead;
    }
}
```
**Python**
```python
def reverse_list(head):
    nodes, cur = [], head
    while cur:
        nodes.append(cur)
        cur = cur.next
    if not nodes: return None
    new_head = nodes[-1]
    cur = new_head
    for node in reversed(nodes[:-1]):
        cur.next = node
        cur = node
    cur.next = None
    return new_head
```
Time `O(n)`, space `O(n)`.

### Best – Three pointers
Save next node, reverse current link, advance previous/current.

### Step-by-step dry run
1. prev=null, cur=1; save next=2 and set 1.next=null.
2. Move prev=1, cur=2; save next=3 and set 2.next=1.
3. Move prev=2, cur=3; save next=null and set 3.next=2.
4. Stop and return prev (node 3).



**Java**
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null, cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
}
```
**Python**
```python
def reverse_list(head):
    prev, cur = None, head
    while cur:
        nxt = cur.next
        cur.next = prev
        prev = cur
        cur = nxt
    return prev
```
Time `O(n)`, space `O(1)`.

### Alternative – Recursion
### Step-by-step dry run
1. Calls move from node 1 to 2 to 3; node 3 is base case and returned as new head.
2. At node 2, make 3.next=2 and set 2.next=null.
3. At node 1, make 2.next=1 and set 1.next=null.
4. Return node 3.



**Java**
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```
**Python**
```python
def reverse_list(head):
    if head is None or head.next is None: return head
    new_head = reverse_list(head.next)
    head.next.next = head
    head.next = None
    return new_head
```
Time `O(n)`, recursion stack `O(n)`.

## 8.2 Detect Cycle

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A list that may loop forever suggests visited-node tracking; optimize with Floyd's slow and fast pointers

### 1. Problem statement
Determine whether a singly linked list contains a cycle.

### 2. Input and output format
- **Input:** `head = [3,2,0,-4], tail connects to index 1`
- **Output:** `true`

### 3. Constraints
0 <= number of nodes <= 10^4; a cycle may connect any node back to an earlier node.

### 4. Examples
- **Example:** `1 → 2 → 3 → null → false`
- **Additional Example 2:** Input `1→2→3→null` → Output `false`
- **Additional Example 3:** Input `1→1 (self-loop)` → Output `true`


### 5. Pattern identification — how to recognize the problem
A list that may loop forever suggests visited-node tracking; optimize with Floyd's slow and fast pointers.

**Pattern:** Floyd’s Slow/Fast Pointers. Determine whether a node eventually points back to an earlier node.

### Easy – Hash Set
### Step-by-step dry run
1. Visit node 3 and store its reference.
2. Visit 2 and store it.
3. Visit 0 and store it.
4. Visit -4 and store it.
5. Its next points to already-seen node 2; return true.



**Java**
```java
import java.util.*;
class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<>();
        while (head != null) {
            if (!seen.add(head)) return true;
            head = head.next;
        }
        return false;
    }
}
```
**Python**
```python
def has_cycle(head):
    seen = set()
    while head:
        if head in seen: return True
        seen.add(head)
        head = head.next
    return False
```
Time `O(n)`, space `O(n)`.

### Best – Floyd algorithm
Slow moves one node, fast moves two. If they meet, cycle exists.

### Step-by-step dry run
1. Slow=fast=node
3.
2. First round: slow→2; fast→0.
3. Second round: slow→0; fast→-4→2.
4. Third round: slow→-4; fast→0→-4.
5. They meet; return true.



**Java**
```java
class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) return true;
        }
        return false;
    }
}
```
**Python**
```python
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast: return True
    return False
```
Time `O(n)`, space `O(1)`. Test empty, one node, self-loop.

## 8.3 Merge Two Sorted Lists

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Two already-sorted linked lists can be merged by repeatedly choosing the smaller head

### 1. Problem statement
Merge two sorted singly linked lists into one sorted linked list by reusing their nodes.

### 2. Input and output format
- **Input:** `list1 = 1 → 2 → 4; list2 = 1 → 3 → 4`
- **Output:** `1 → 1 → 2 → 3 → 4 → 4`

### 3. Constraints
Each list may be empty; values are sorted in non-decreasing order.

### 4. Examples
- **Example:** `1 → 3 and 2 → 4 → 1 → 2 → 3 → 4`
- **Additional Example 2:** Input `list1=null, list2=0→2` → Output `0→2`
- **Additional Example 3:** Input `list1=1→3, list2=2→4` → Output `1→2→3→4`


### 5. Pattern identification — how to recognize the problem
Two already-sorted linked lists can be merged by repeatedly choosing the smaller head. A dummy node simplifies pointer handling.

**Pattern:** Two Pointers + Dummy Node. Merge two ascending linked lists.

### Easy – Recursive
### Step-by-step dry run
1. Compare heads 1 and 1; choose first list's
1.
2. Compare 2 and 1; choose second list's
1.
3. Compare 2 and 3; choose
2.
4. Compare 4 and 3; choose
3.
5. Compare 4 and 4; choose one 4, then attach the remaining
4. Result 1→1→2→3→4→4.



**Java**
```java
class Solution {
    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        if (a == null) return b;
        if (b == null) return a;
        if (a.val <= b.val) {
            a.next = mergeTwoLists(a.next, b);
            return a;
        }
        b.next = mergeTwoLists(a, b.next);
        return b;
    }
}
```
**Python**
```python
def merge_two_lists(a, b):
    if a is None: return b
    if b is None: return a
    if a.val <= b.val:
        a.next = merge_two_lists(a.next, b)
        return a
    b.next = merge_two_lists(a, b.next)
    return b
```
Time `O(m+n)`, recursion stack `O(m+n)` worst case.

### Best – Iterative dummy node
### Step-by-step dry run
1. Dummy tail starts before both lists.
2. Compare 1 and 1; attach first list's
1.
3. Compare 2 and 1; attach second list's
1.
4. Attach 2, then
3.
5. Attach one 4 and then the remaining
4. Return dummy.next.



**Java**
```java
class Solution {
    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        ListNode dummy = new ListNode(0), tail = dummy;
        while (a != null && b != null) {
            if (a.val <= b.val) { tail.next = a; a = a.next; }
            else { tail.next = b; b = b.next; }
            tail = tail.next;
        }
        tail.next = (a != null) ? a : b;
        return dummy.next;
    }
}
```
**Python**
```python
def merge_two_lists(a, b):
    dummy = ListNode(0)
    tail = dummy
    while a and b:
        if a.val <= b.val:
            tail.next = a
            a = a.next
        else:
            tail.next = b
            b = b.next
        tail = tail.next
    tail.next = a if a else b
    return dummy.next
```
Time `O(m+n)`, auxiliary space `O(1)` (reuses nodes).

---

# 9. STACK

## 9.1 Valid Parentheses

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Nested matching requires last-opened, first-closed behavior: Stack

### 1. Problem statement
Determine whether every opening bracket has a matching closing bracket in the correct order.

### 2. Input and output format
- **Input:** `s = '({[]})'`
- **Output:** `true`

### 3. Constraints
1 <= s.length <= 10^4; characters are only (), [], {}.

### 4. Examples
- **Example:** `'([)]' → false`
- **Additional Example 2:** Input `s='([)]'` → Output `false`
- **Additional Example 3:** Input `s='((()))'` → Output `true`


### 5. Pattern identification — how to recognize the problem
Nested matching requires last-opened, first-closed behavior: Stack. Repeated replacement is a simple baseline.

**Pattern:** Stack. Brackets must match and be correctly nested.

```text
Input: "({[]})" → true
```

### Easy – Repeated replacement
Repeatedly remove `()`, `[]`, `{}` until no change.

### Step-by-step dry run
1. Start '({[]})'.
2. Remove inner '[]' → '({})'.
3. Remove '{}' → '()'.
4. Remove '()' → ''.
5. Empty string means valid; return true.



**Java**
```java
class Solution {
    public boolean isValid(String s) {
        while (s.contains("()") || s.contains("[]") || s.contains("{}"))
            s = s.replace("()", "").replace("[]", "").replace("{}", "");
        return s.isEmpty();
    }
}
```
**Python**
```python
def is_valid(s):
    while "()" in s or "[]" in s or "{}" in s:
        s = s.replace("()", "").replace("[]", "").replace("{}", "")
    return s == ""
```
Worst-case `O(n²)` due to repeated scanning/copying.

### Best – Stack
Push open brackets. For each close bracket, check and pop its matching open bracket.

### Step-by-step dry run
1. Read '('; push.
2. Read '{'; push.
3. Read '['; push.
4. Read ']'; top '[' matches, pop.
5. Read '}'; top '{' matches, pop.
6. Read ')'; top '(' matches, pop.
7. Stack is empty; return true.



**Java**
```java
import java.util.*;
class Solution {
    public boolean isValid(String s) {
        Deque<Character> st = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (c == '(' || c == '[' || c == '{') st.push(c);
            else {
                if (st.isEmpty()) return false;
                char x = st.pop();
                if ((c == ')' && x != '(') ||
                    (c == ']' && x != '[') ||
                    (c == '}' && x != '{')) return false;
            }
        }
        return st.isEmpty();
    }
}
```
**Python**
```python
def is_valid(s):
    stack = []
    pairs = {')': '(', ']': '[', '}': '{'}
    for c in s:
        if c in "([{":
            stack.append(c)
        else:
            if not stack or stack.pop() != pairs[c]: return False
    return not stack
```
Time `O(n)`, space `O(n)`. Test wrong nesting, closing-first, leftover opening.

## 9.2 Next Greater Element

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** The next greater value to the right suggests a monotonic decreasing stack

### 1. Problem statement
For every array element, find the first strictly greater element to its right; use -1 if none exists.

### 2. Input and output format
- **Input:** `a = [4,5,2,10]`
- **Output:** `[5,10,10,-1]`

### 3. Constraints
1 <= a.length <= 10^4; equal values are not greater.

### 4. Examples
- **Example:** `[2,2,3] → [3,3,-1]`
- **Additional Example 2:** Input `a=[2,2,3]` → Output `[3,3,-1]`
- **Additional Example 3:** Input `a=[5,4,3]` → Output `[-1,-1,-1]`


### 5. Pattern identification — how to recognize the problem
The next greater value to the right suggests a monotonic decreasing stack. Nested scanning is the baseline.

**Pattern:** Monotonic Decreasing Stack. For each element, find first greater value to its right; if none, return `-1`.

```text
Input: [4,5,2,10] → [5,10,10,-1]
```

### Easy – Nested scan
### Step-by-step dry run
1. For 4, scan right: 5 is the first greater value.
2. For 5, skip 2 and find
10.
3. For 2, find
10.
4. For 10, no value remains, so -1. Output [5,10,10,-1].



**Java**
```java
import java.util.*;
class Solution {
    public int[] nextGreater(int[] a) {
        int[] ans = new int[a.length];
        Arrays.fill(ans, -1);
        for (int i = 0; i < a.length; i++)
            for (int j = i + 1; j < a.length; j++)
                if (a[j] > a[i]) { ans[i] = a[j]; break; }
        return ans;
    }
}
```
**Python**
```python
def next_greater(a):
    ans = [-1] * len(a)
    for i in range(len(a)):
        for j in range(i + 1, len(a)):
            if a[j] > a[i]:
                ans[i] = a[j]
                break
    return ans
```
Time `O(n²)`.

### Best – Monotonic stack of indices
When current value is greater than the value at the top index, it resolves that index’s answer.

### Step-by-step dry run
1. Read 4; push index 0.
2. Read 5; 5>4, pop index 0 and set answer[0]=5; push
1.
3. Read 2; push index
2.
4. Read 10; pop index 2 and set 10, then pop index 1 and set 10; push
3.
5. Index 3 remains -1.



**Java**
```java
import java.util.*;
class Solution {
    public int[] nextGreater(int[] a) {
        int[] ans = new int[a.length];
        Arrays.fill(ans, -1);
        Deque<Integer> st = new ArrayDeque<>();
        for (int i = 0; i < a.length; i++) {
            while (!st.isEmpty() && a[i] > a[st.peek()])
                ans[st.pop()] = a[i];
            st.push(i);
        }
        return ans;
    }
}
```
**Python**
```python
def next_greater(a):
    ans = [-1] * len(a)
    stack = []
    for i, x in enumerate(a):
        while stack and x > a[stack[-1]]:
            ans[stack.pop()] = x
        stack.append(i)
    return ans
```
Time `O(n)` (each index pushed/popped once), space `O(n)`. Equal values are not greater.

---

# 10. QUEUE

A queue follows FIFO: First In, First Out. Enqueue adds at rear; dequeue removes from front.

## 10.1 Implement Queue

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** FIFO means first inserted is first removed

### 1. Problem statement
Implement FIFO queue operations: enqueue, dequeue, peek/front, and isEmpty.

### 2. Input and output format
- **Input:** `enqueue(10), enqueue(20), dequeue(), peek()`
- **Output:** `dequeue returns 10; peek returns 20`

### 3. Constraints
Handle empty dequeue/peek; define capacity if using a fixed-size array.

### 4. Examples
- **Example:** `enqueue 1, enqueue 2, dequeue → 1`
- **Additional Example 2:** Input `enqueue(1), enqueue(2), dequeue()` → Output `dequeue returns 1; front=2`
- **Additional Example 3:** Input `enqueue(7), peek(), dequeue()` → Output `peek=7; dequeue=7`


### 5. Pattern identification — how to recognize the problem
FIFO means first inserted is first removed. Shifting arrays are simple; linked-list front/rear pointers give O(1) operations.

Operations: `enqueue`, `dequeue`, `peek`, `isEmpty`.

### Easy – Array with shifting
### Step-by-step dry run
1. Enqueue 10 → [10].
2. Enqueue 20 → [10,20].
3. Dequeue returns 10; shift 20 to index 0 → [20].
4. Peek returns 20.



**Java**
```java
class SimpleQueue {
    int[] a = new int[1000];
    int size = 0;
    void enqueue(int x) { a[size++] = x; }
    int dequeue() {
        if (size == 0) throw new RuntimeException("Empty");
        int x = a[0];
        for (int i = 1; i < size; i++) a[i-1] = a[i];
        size--;
        return x;
    }
    int peek() {
        if (size == 0) throw new RuntimeException("Empty");
        return a[0];
    }
    boolean isEmpty() { return size == 0; }
}
```
**Python**
```python
class SimpleQueue:
    def __init__(self): self.data = []
    def enqueue(self, x): self.data.append(x)
    def dequeue(self):
        if not self.data: raise IndexError("Empty")
        return self.data.pop(0)
    def peek(self):
        if not self.data: raise IndexError("Empty")
        return self.data[0]
    def is_empty(self): return len(self.data) == 0
```
Enqueue `O(1)` amortized; dequeue `O(n)` because shifting / front pop.

### Best – Linked-list queue
### Step-by-step dry run
1. Enqueue 10: front=rear=node10.
2. Enqueue 20: node10.next=node20 and rear=node20.
3. Dequeue returns 10 and moves front to node20.
4. Peek returns 20.
5. Removing the last node sets both front and rear to null.



**Java**
```java
class MyQueue {
    static class Node {
        int val; Node next;
        Node(int v) { val = v; }
    }
    Node front, rear;
    void enqueue(int x) {
        Node node = new Node(x);
        if (rear == null) front = rear = node;
        else { rear.next = node; rear = node; }
    }
    int dequeue() {
        if (front == null) throw new RuntimeException("Empty");
        int x = front.val;
        front = front.next;
        if (front == null) rear = null;
        return x;
    }
    int peek() {
        if (front == null) throw new RuntimeException("Empty");
        return front.val;
    }
    boolean isEmpty() { return front == null; }
}
```
**Python**
```python
class Node:
    def __init__(self, val):
        self.val, self.next = val, None

class MyQueue:
    def __init__(self): self.front = self.rear = None
    def enqueue(self, x):
        node = Node(x)
        if self.rear is None: self.front = self.rear = node
        else:
            self.rear.next = node
            self.rear = node
    def dequeue(self):
        if self.front is None: raise IndexError("Empty")
        x = self.front.val
        self.front = self.front.next
        if self.front is None: self.rear = None
        return x
    def peek(self):
        if self.front is None: raise IndexError("Empty")
        return self.front.val
    def is_empty(self): return self.front is None
```
Enqueue/dequeue/peek `O(1)`, storage `O(n)`. In real Java code, use `Queue<Integer>` with `ArrayDeque` unless asked to implement it.

## 10.2 Circular Queue

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A fixed-size queue that wraps around uses modulo: (index + 1) % capacity

### 1. Problem statement
Implement a fixed-capacity queue that reuses array positions by wrapping indices around.

### 2. Input and output format
- **Input:** `capacity=3; enqueue 1,2,3; dequeue; enqueue 4`
- **Output:** `front=2, rear=4; queue is full`

### 3. Constraints
Capacity is positive; enqueue fails when full and dequeue fails when empty.

### 4. Examples
- **Example:** `capacity=2: enqueue 1,2, dequeue, enqueue 3 → front 2, rear 3`
- **Additional Example 2:** Input `capacity=2; enQueue(1), enQueue(2), deQueue(), enQueue(3)` → Output `front=2, rear=3; full`
- **Additional Example 3:** Input `capacity=1; enQueue(9), enQueue(8), deQueue()` → Output `true, false, true`


### 5. Pattern identification — how to recognize the problem
A fixed-size queue that wraps around uses modulo: (index + 1) % capacity.

**Pattern:** Modulo indexing. Reuse fixed array positions. Maintain `front`, `rear`, `size`, `capacity`.

```text
next index = (current index + 1) % capacity
empty when size == 0; full when size == capacity
```

### Step-by-step dry run
Capacity=3; initially front=0,rear=-1,size=0.
1. Enqueue 10: rear=(−1+1)%3=0; size=1.
2. Enqueue 20: rear=1; size=2.
3. Enqueue 30: rear=2; size=3 (full).
4. Dequeue: front=(0+1)%3=1; size=2.
5. Enqueue 40: rear=(2+1)%3=0; store at index 0; size=3.
6. Front=20, rear=40; wraparound works.



**Java**
```java
class MyCircularQueue {
    int[] a;
    int front = 0, rear = -1, size = 0, capacity;
    MyCircularQueue(int k) { a = new int[k]; capacity = k; }

    boolean enQueue(int value) {
        if (isFull()) return false;
        rear = (rear + 1) % capacity;
        a[rear] = value;
        size++;
        return true;
    }
    boolean deQueue() {
        if (isEmpty()) return false;
        front = (front + 1) % capacity;
        size--;
        return true;
    }
    int Front() { return isEmpty() ? -1 : a[front]; }
    int Rear() { return isEmpty() ? -1 : a[rear]; }
    boolean isEmpty() { return size == 0; }
    boolean isFull() { return size == capacity; }
}
```
**Python**
```python
class MyCircularQueue:
    def __init__(self, k):
        self.a = [0] * k
        self.capacity = k
        self.front = 0
        self.rear = -1
        self.size = 0

    def enQueue(self, value):
        if self.isFull(): return False
        self.rear = (self.rear + 1) % self.capacity
        self.a[self.rear] = value
        self.size += 1
        return True

    def deQueue(self):
        if self.isEmpty(): return False
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return True

    def Front(self): return -1 if self.isEmpty() else self.a[self.front]
    def Rear(self): return -1 if self.isEmpty() else self.a[self.rear]
    def isEmpty(self): return self.size == 0
    def isFull(self): return self.size == self.capacity
```
All operations `O(1)`, space `O(k)`. Test capacity 1, full queue, empty queue, and wraparound.

---

# 11. RECURSION

Every recursive function needs a base case and progress toward that base case.

## 11.1 Factorial

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** A problem defined in terms of n times the answer for n-1 is naturally recursive; iteration is the constant-stack alternative

### 1. Problem statement
Return n factorial, the product of all positive integers from 1 through n; 0! is 1.

### 2. Input and output format
- **Input:** `n = 5`
- **Output:** `120`

### 3. Constraints
n >= 0; result type must be large enough for the chosen n.

### 4. Examples
- **Example:** `n = 0 → 1`
- **Additional Example 2:** Input `n=0` → Output `1`
- **Additional Example 3:** Input `n=6` → Output `720`


### 5. Pattern identification — how to recognize the problem
A problem defined in terms of n times the answer for n-1 is naturally recursive; iteration is the constant-stack alternative.

`n! = n × (n-1) × ... × 1`, and `0! = 1`. Example `5! = 120`.

### Easy – Iterative
### Step-by-step dry run
1. Start result=1.
2. Multiply by 2 →
2.
3. Multiply by 3 →
6.
4. Multiply by 4 → 24.
5. Multiply by 5 → 120.



**Java**
```java
class Solution {
    public long factorial(int n) {
        long ans = 1;
        for (int i = 2; i <= n; i++) ans *= i;
        return ans;
    }
}
```
**Python**
```python
def factorial(n):
    ans = 1
    for i in range(2, n + 1): ans *= i
    return ans
```

### Recursive
### Step-by-step dry run
1. factorial(5) calls 5×factorial(4).
2. Calls continue through 4×factorial(3), 3×factorial(2), 2×factorial(1).
3. Base case factorial(1)=1.
4. Return values upward: 2, 6, 24, 120.



**Java**
```java
class Solution {
    public long factorial(int n) {
        if (n <= 1) return 1;
        return n * factorial(n - 1);
    }
}
```
**Python**
```python
def factorial(n):
    if n <= 1: return 1
    return n * factorial(n - 1)
```
Time `O(n)`; iterative space `O(1)`, recursive stack `O(n)`. `long` overflows for sufficiently large n.

## 11.2 Fibonacci

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Repeated overlapping subproblems identify Fibonacci DP

### 1. Problem statement
Return the nth Fibonacci number, where F(0)=0, F(1)=1, and F(n)=F(n-1)+F(n-2).

### 2. Input and output format
- **Input:** `n = 6`
- **Output:** `8`

### 3. Constraints
n >= 0; choose a numeric type that can hold the result.

### 4. Examples
- **Example:** `n = 4 → 3`
- **Additional Example 2:** Input `n=0` → Output `0`
- **Additional Example 3:** Input `n=7` → Output `13`


### 5. Pattern identification — how to recognize the problem
Repeated overlapping subproblems identify Fibonacci DP. Compare naive recursion, memoization, and iterative two-variable DP.

`F(0)=0`, `F(1)=1`, `F(n)=F(n-1)+F(n-2)`. Example: `0,1,1,2,3,5,8`.

### Easy – Naive recursion
### Step-by-step dry run
1. fib(4)=fib(3)+fib(2).
2. fib(3)=fib(2)+fib(1)=2.
3. fib(2)=fib(1)+fib(0)=1.
4. fib(4)=2+1=3; note fib(2) is recalculated.



**Java**
```java
class Solution {
    public int fib(int n) {
        if (n <= 1) return n;
        return fib(n - 1) + fib(n - 2);
    }
}
```
**Python**
```python
def fib(n):
    if n <= 1: return n
    return fib(n - 1) + fib(n - 2)
```
Time `O(2^n)`, stack `O(n)`.

### Better – Memoization
### Step-by-step dry run
1. fib(4) requests fib(3) and fib(2).
2. Compute and store fib(2)=1.
3. Compute/store fib(3)=fib(2)+fib(1)=2.
4. fib(4)=2+1=3; reuse stored values when requested.



**Java**
```java
import java.util.*;
class Solution {
    public int fib(int n) {
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);
        return solve(n, memo);
    }
    private int solve(int n, int[] memo) {
        if (n <= 1) return n;
        if (memo[n] != -1) return memo[n];
        return memo[n] = solve(n-1, memo) + solve(n-2, memo);
    }
}
```
**Python**
```python
def fib(n):
    memo = {}
    def solve(x):
        if x <= 1: return x
        if x not in memo:
            memo[x] = solve(x-1) + solve(x-2)
        return memo[x]
    return solve(n)
```
Time `O(n)`, space `O(n)`.

### Best – Iterative two variables
### Step-by-step dry run
1. Start a=0,b=1.
2. i=2: next=1; now a=1,b=1.
3. i=3: next=2; now a=1,b=2.
4. i=4: next=3; now a=2,b=3. Return 3.



**Java**
```java
class Solution {
    public int fib(int n) {
        if (n <= 1) return n;
        int a = 0, b = 1;
        for (int i = 2; i <= n; i++) {
            int next = a + b;
            a = b; b = next;
        }
        return b;
    }
}
```
**Python**
```python
def fib(n):
    if n <= 1: return n
    a, b = 0, 1
    for _ in range(2, n + 1): a, b = b, a + b
    return b
```
Time `O(n)`, space `O(1)`.

## 11.3 Subsets

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Every element is either included or excluded

### 1. Problem statement
Generate every possible subset of an input array of distinct values, including the empty set.

### 2. Input and output format
- **Input:** `nums = [1,2]`
- **Output:** `[[],[1],[2],[1,2]]`

### 3. Constraints
0 <= nums.length <= 10; input values are distinct in this version.

### 4. Examples
- **Example:** `nums = [0] → [[],[0]]`
- **Additional Example 2:** Input `nums=[0]` → Output `[[],[0]]`
- **Additional Example 3:** Input `nums=[1,2,3]` → Output `8 subsets`


### 5. Pattern identification — how to recognize the problem
Every element is either included or excluded. This binary choice suggests recursion/backtracking; iterative expansion is an easy alternative.

**Pattern:** Backtracking / Include-Exclude. Generate all subsets of distinct input values.

```text
Input: [1,2] → [[], [1], [2], [1,2]]
```

### Easy – Iterative expansion
For each value, append it to a copy of every subset already created.

### Step-by-step dry run
1. Start result=[[]].
2. Process 1: add a copy of [] with 1 → [[],[1]].
3. Process 2: extend [] to [2] and [1] to [1,2].
4. Result=[[],[1],[2],[1,2]].



**Java**
```java
import java.util.*;
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        result.add(new ArrayList<>());
        for (int x : nums) {
            int size = result.size();
            for (int i = 0; i < size; i++) {
                List<Integer> next = new ArrayList<>(result.get(i));
                next.add(x);
                result.add(next);
            }
        }
        return result;
    }
}
```
**Python**
```python
def subsets(nums):
    result = [[]]
    for x in nums:
        result += [s + [x] for s in result]
    return result
```

### Best – Recursive backtracking
At each index, explore excluding and including the current element.

### Step-by-step dry run
1. At value 1, explore exclude branch.
2. At value 2 in that branch, exclude → []; include → [2].
3. Backtrack and include
1.
4. At value 2, exclude → [1]; include → [1,2].
5. Four leaves represent all subsets.



**Java**
```java
import java.util.*;
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(0, nums, new ArrayList<>(), result);
        return result;
    }
    private void backtrack(int i, int[] nums, List<Integer> path,
                           List<List<Integer>> result) {
        if (i == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }
        backtrack(i + 1, nums, path, result);
        path.add(nums[i]);
        backtrack(i + 1, nums, path, result);
        path.remove(path.size() - 1);
    }
}
```
**Python**
```python
def subsets(nums):
    result = []
    def backtrack(i, path):
        if i == len(nums):
            result.append(path[:])
            return
        backtrack(i + 1, path)
        path.append(nums[i])
        backtrack(i + 1, path)
        path.pop()
    backtrack(0, [])
    return result
```
Time `O(n * 2^n)` to build output; output space `O(n * 2^n)`. Exactly `2^n` subsets.

---

# 12. TREES

Use this node definition:

**Java**
```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}
```
**Python**
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right
```

Sample tree:
```text
        1
       / \
      2   3
     / \
    4   5
```

## 12.1 Inorder – Left, Root, Right

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** The requested visit order is Left-Root-Right

### 1. Problem statement
Return a binary tree's values in inorder: visit left subtree, node, then right subtree.

### 2. Input and output format
- **Input:** `Tree: root 1; left 2 with children 4,5; right 3`
- **Output:** `[4,2,5,1,3]`

### 3. Constraints
0 <= number of nodes <= 100; node values may repeat.

### 4. Examples
- **Example:** `Single node 7 → [7]`
- **Additional Example 2:** Input `tree=[1,null,2,3]` → Output `[1,3,2]`
- **Additional Example 3:** Input `tree=[7]` → Output `[7]`


### 5. Pattern identification — how to recognize the problem
The requested visit order is Left-Root-Right. Recursive DFS is simplest; an explicit stack removes recursion.

Expected: `[4,2,5,1,3]`.

### Recursive
### Step-by-step dry run
1. From root 1, recurse left to
2.
2. Recurse left to 4 and visit
4.
3. Return and visit
2.
4. Visit 2's right child
5.
5. Return to root and visit
1.
6. Visit right child
3. Output [4,2,5,1,3].



**Java**
```java
import java.util.*;
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        inorder(root, ans);
        return ans;
    }
    private void inorder(TreeNode node, List<Integer> ans) {
        if (node == null) return;
        inorder(node.left, ans);
        ans.add(node.val);
        inorder(node.right, ans);
    }
}
```
**Python**
```python
def inorder(root):
    ans = []
    def visit(node):
        if not node: return
        visit(node.left)
        ans.append(node.val)
        visit(node.right)
    visit(root)
    return ans
```

### Iterative – Stack
### Step-by-step dry run
1. Push 1, then 2, then 4 while moving left.
2. Pop/visit
4.
3. Pop/visit 2; move to its right child
5.
4. Visit
5.
5. Pop/visit 1; move right to
3.
6. Visit
3. Output [4,2,5,1,3].



**Java**
```java
import java.util.*;
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        Deque<TreeNode> st = new ArrayDeque<>();
        TreeNode cur = root;
        while (cur != null || !st.isEmpty()) {
            while (cur != null) { st.push(cur); cur = cur.left; }
            cur = st.pop();
            ans.add(cur.val);
            cur = cur.right;
        }
        return ans;
    }
}
```
**Python**
```python
def inorder(root):
    ans, st, cur = [], [], root
    while cur or st:
        while cur:
            st.append(cur)
            cur = cur.left
        cur = st.pop()
        ans.append(cur.val)
        cur = cur.right
    return ans
```

## 12.2 Preorder – Root, Left, Right

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Root is visited first, then left and right

### 1. Problem statement
Return a binary tree's values in preorder: visit node, left subtree, then right subtree.

### 2. Input and output format
- **Input:** `Tree: root 1; left 2 with children 4,5; right 3`
- **Output:** `[1,2,4,5,3]`

### 3. Constraints
0 <= number of nodes <= 100; node values may repeat.

### 4. Examples
- **Example:** `Single node 7 → [7]`
- **Additional Example 2:** Input `tree=[1,null,2,3]` → Output `[1,2,3]`
- **Additional Example 3:** Input `tree=[7]` → Output `[7]`


### 5. Pattern identification — how to recognize the problem
Root is visited first, then left and right. Use recursive DFS or a stack; push right before left.

Expected: `[1,2,4,5,3]`.

### Recursive
### Step-by-step dry run
1. Visit root 1 first.
2. Enter left subtree and visit
2.
3. Visit 2's left child 4, then right child
5.
4. Return and visit root's right child
3. Output [1,2,4,5,3].



**Java**
```java
import java.util.*;
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        preorder(root, ans);
        return ans;
    }
    private void preorder(TreeNode node, List<Integer> ans) {
        if (node == null) return;
        ans.add(node.val);
        preorder(node.left, ans);
        preorder(node.right, ans);
    }
}
```
**Python**
```python
def preorder(root):
    ans = []
    def visit(node):
        if not node: return
        ans.append(node.val)
        visit(node.left)
        visit(node.right)
    visit(root)
    return ans
```

### Iterative – Stack (push right before left)
### Step-by-step dry run
1. Push 1; pop and visit
1.
2. Push right child 3, then left child
2.
3. Pop/visit 2; push 5 then
4.
4. Pop/visit 4, then 5, then
3. Output [1,2,4,5,3].



**Java**
```java
import java.util.*;
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if (root == null) return ans;
        Deque<TreeNode> st = new ArrayDeque<>();
        st.push(root);
        while (!st.isEmpty()) {
            TreeNode node = st.pop();
            ans.add(node.val);
            if (node.right != null) st.push(node.right);
            if (node.left != null) st.push(node.left);
        }
        return ans;
    }
}
```
**Python**
```python
def preorder(root):
    if not root: return []
    ans, st = [], [root]
    while st:
        node = st.pop()
        ans.append(node.val)
        if node.right: st.append(node.right)
        if node.left: st.append(node.left)
    return ans
```

## 12.3 Postorder – Left, Right, Root

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** Root is visited last

### 1. Problem statement
Return a binary tree's values in postorder: visit left subtree, right subtree, then node.

### 2. Input and output format
- **Input:** `Tree: root 1; left 2 with children 4,5; right 3`
- **Output:** `[4,5,2,3,1]`

### 3. Constraints
0 <= number of nodes <= 100; node values may repeat.

### 4. Examples
- **Example:** `Single node 7 → [7]`
- **Additional Example 2:** Input `tree=[1,null,2,3]` → Output `[3,2,1]`
- **Additional Example 3:** Input `tree=[7]` → Output `[7]`


### 5. Pattern identification — how to recognize the problem
Root is visited last. Recursive DFS is direct; iterative traversal can reverse a root-right-left ordering.

Expected: `[4,5,2,3,1]`.

### Recursive
### Step-by-step dry run
1. From root 1, recurse left to
2.
2. Visit 2's left child 4, then right child
5.
3. Visit node 2 after both children.
4. Visit root's right child
3.
5. Visit root 1 last. Output [4,5,2,3,1].



**Java**
```java
import java.util.*;
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        postorder(root, ans);
        return ans;
    }
    private void postorder(TreeNode node, List<Integer> ans) {
        if (node == null) return;
        postorder(node.left, ans);
        postorder(node.right, ans);
        ans.add(node.val);
    }
}
```
**Python**
```python
def postorder(root):
    ans = []
    def visit(node):
        if not node: return
        visit(node.left)
        visit(node.right)
        ans.append(node.val)
    visit(root)
    return ans
```

### Iterative – Two stacks / reverse order
### Step-by-step dry run
1. Pop root 1 and record it; push children.
2. Process node 3, then node
2.
3. Process 2's children 4 and
5.
4. Recorded order is [1,3,2,5,4].
5. Reverse it → [4,5,2,3,1].



**Java**
```java
import java.util.*;
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if (root == null) return ans;
        Deque<TreeNode> st = new ArrayDeque<>();
        st.push(root);
        while (!st.isEmpty()) {
            TreeNode node = st.pop();
            ans.add(node.val);
            if (node.left != null) st.push(node.left);
            if (node.right != null) st.push(node.right);
        }
        Collections.reverse(ans);
        return ans;
    }
}
```
**Python**
```python
def postorder(root):
    if not root: return []
    ans, st = [], [root]
    while st:
        node = st.pop()
        ans.append(node.val)
        if node.left: st.append(node.left)
        if node.right: st.append(node.right)
    return ans[::-1]
```

## 12.4 Level Order – BFS with Queue

**Difficulty:** Interview practice (Easy–Medium; varies by platform)  
**Pattern:** The phrase level by level signals breadth-first search using a queue; process the queue's current size for each level

### 1. Problem statement
Return the binary tree values level by level from top to bottom, left to right.

### 2. Input and output format
- **Input:** `Tree: root 1; left 2 with children 4,5; right 3`
- **Output:** `[[1],[2,3],[4,5]]`

### 3. Constraints
0 <= number of nodes <= 2000; empty tree returns an empty list.

### 4. Examples
- **Example:** `Single node 7 → [[7]]`
- **Additional Example 2:** Input `tree=[1,null,2,3]` → Output `[[1],[2],[3]]`
- **Additional Example 3:** Input `tree=[7]` → Output `[[7]]`


### 5. Pattern identification — how to recognize the problem
The phrase level by level signals breadth-first search using a queue; process the queue's current size for each level.

Expected: `[[1], [2,3], [4,5]]`.

### Step-by-step dry run
1. Enqueue root
1.
2. Level 1 has queue size 1: remove 1, record [1], enqueue 2 and
3.
3. Level 2 has size 2: remove 2 (enqueue 4,5), then remove 3; record [2,3].
4. Level 3 has size 2: remove 4 and 5; record [4,5].
5. Queue is empty; output [[1],[2,3],[4,5]].



**Java**
```java
import java.util.*;
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) return ans;
        Queue<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        while (!q.isEmpty()) {
            int size = q.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                level.add(node.val);
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
            ans.add(level);
        }
        return ans;
    }
}
```
**Python**
```python
from collections import deque
def level_order(root):
    if not root: return []
    ans, q = [], deque([root])
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left: q.append(node.left)
            if node.right: q.append(node.right)
        ans.append(level)
    return ans
```

All traversals take `O(n)` time. DFS recursion uses `O(h)` stack; iterative DFS uses stack space up to `O(n)`; BFS queue uses up to `O(w)` where `w` is maximum tree width.

### Traversal memory table

| Traversal | Order | Sample |
|---|---|---|
| Inorder | Left → Root → Right | 4, 2, 5, 1, 3 |
| Preorder | Root → Left → Right | 1, 2, 4, 5, 3 |
| Postorder | Left → Right → Root | 4, 5, 2, 3, 1 |
| Level order | Level by level | 1 / 2,3 / 4,5 |

---

# 13. PATTERN RECOGNITION CHEAT SHEET

| Question clue | Think of this pattern |
|---|---|
| Pair equals target | Hash Map; Two Pointers if sorted |
| Move elements in place | Read/write pointers |
| Maximum contiguous sum | Kadane |
| Missing value from 0..n | Sum formula / XOR |
| Same letters/frequencies | Sorting / Frequency Map |
| Count occurrences | Hash Map |
| Detect duplicates | Hash Set |
| Search sorted data | Binary Search |
| First/last duplicate position | Boundary Binary Search |
| Adjacent swaps | Bubble Sort |
| Insert into sorted prefix | Insertion Sort |
| Repeatedly select minimum | Selection Sort |
| Divide and merge | Merge Sort |
| Pivot and partition | Quick Sort |
| Pair in sorted array | Two Pointers |
| Unique values in sorted array | Slow/Fast Pointers |
| Fixed contiguous range | Fixed Sliding Window |
| Longest substring satisfying condition | Variable Sliding Window |
| Reverse node links | Three Pointers |
| Detect linked-list cycle | Floyd Slow/Fast |
| Merge sorted lists | Dummy Node + Two Pointers |
| Matching nested brackets | Stack |
| Next larger value to right | Monotonic Stack |
| FIFO operations | Queue |
| Reuse fixed array positions | Circular Queue + Modulo |
| Repeated smaller subproblem | Recursion / DP |
| Generate every choice | Backtracking |
| Visit tree nodes | DFS / BFS |

# 14. COMPLEXITY QUICK REFERENCE

| Algorithm / operation | Typical time | Typical extra space |
|---|---:|---:|
| Array index access | O(1) | O(1) |
| Linear search | O(n) | O(1) |
| Binary search | O(log n) | O(1) iterative |
| Hash lookup/insert | O(1) average | O(n) structure |
| Bubble/Selection/Insertion | O(n²) average | O(1) |
| Merge sort | O(n log n) | O(n) |
| Quick sort | O(n log n) average, O(n²) worst | O(log n) average stack |
| Stack push/pop | O(1) | O(n) storage |
| Queue enqueue/dequeue | O(1) suitable implementation | O(n) storage |
| Linked-list traversal | O(n) | O(1) iterative |
| Tree traversal | O(n) | O(h) DFS stack / O(w) BFS queue |
| Generate all subsets | O(n × 2ⁿ) | O(n × 2ⁿ) output |

`n` = input size; `k` = number of distinct characters/values; `h` = tree height; `w` = maximum tree width. State whether output space is included when explaining complexity.

# 15. FINAL PRACTICE CHECKLIST

## Arrays and strings
- [ ] Two Sum: brute force and Hash Map.
- [ ] Move Zeroes: in-place without changing order.
- [ ] Maximum Subarray: Kadane, including all-negative input.
- [ ] Missing Number: sum and XOR.
- [ ] Reverse String: two pointers.
- [ ] Valid Palindrome: ignore punctuation and case.
- [ ] Anagram: sorting and frequency count.
- [ ] First Non-Repeating Character: two passes.

## Hashing, searching, sorting and pointers
- [ ] Frequency Count and Duplicate Elements.
- [ ] Iterative Binary Search without off-by-one errors.
- [ ] First and Last Position using two boundary searches.
- [ ] Explain all five sorting algorithms and complexities.
- [ ] Pair Sum on sorted input.
- [ ] Remove Duplicates in-place.

## Sliding window and data structures
- [ ] Fixed-size maximum-sum window.
- [ ] Longest substring without repeating characters.
- [ ] Reverse linked list iteratively.
- [ ] Detect cycle with Floyd’s algorithm.
- [ ] Merge two sorted linked lists.
- [ ] Valid Parentheses using stack.
- [ ] Next Greater Element using monotonic stack.
- [ ] Implement normal and circular queues.

## Recursion and trees
- [ ] Explain base case and recursive case.
- [ ] Compare naive, memoized, and iterative Fibonacci.
- [ ] Generate subsets using backtracking.
- [ ] Write inorder, preorder, and postorder.
- [ ] Write level-order traversal using a queue.

## How to explain any coding answer
1. Restate the problem.
2. Confirm assumptions and constraints.
3. Explain the simple approach.
4. Explain the optimization and chosen data structure.
5. Dry-run a small example.
6. Write clean code.
7. Test edge cases.
8. State time and auxiliary-space complexity.

**Final reminder:** Do not memorize code line by line. Learn the pattern, pointer movement, data-structure choice, and why the algorithm works. That makes it possible to adapt when the interviewer changes the input or constraints.

Expected: `[[1], [2,3], [4,5]]`.

**Java**
```java
import java.util.*;
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) return ans;
        Queue<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        while (!q.isEmpty()) {
            int size = q.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                level.add(node.val);
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
            ans.add(level);
        }
        return ans;
    }
}
```
**Python**
```python
from collections import deque
def level_order(root):
    if not root: return []
    ans, q = [], deque([root])
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left: q.append(node.left)
            if node.right: q.append(node.right)
        ans.append(level)
    return ans
```

All traversals take `O(n)` time. DFS recursion uses `O(h)` stack; iterative DFS uses stack space up to `O(n)`; BFS queue uses up to `O(w)` where `w` is maximum tree width.

### Traversal memory table

| Traversal | Order | Sample |
|---|---|---|
| Inorder | Left → Root → Right | 4, 2, 5, 1, 3 |
| Preorder | Root → Left → Right | 1, 2, 4, 5, 3 |
| Postorder | Left → Right → Root | 4, 5, 2, 3, 1 |
| Level order | Level by level | 1 / 2,3 / 4,5 |

---

# 13. PATTERN RECOGNITION CHEAT SHEET

| Question clue | Think of this pattern |
|---|---|
| Pair equals target | Hash Map; Two Pointers if sorted |
| Move elements in place | Read/write pointers |
| Maximum contiguous sum | Kadane |
| Missing value from 0..n | Sum formula / XOR |
| Same letters/frequencies | Sorting / Frequency Map |
| Count occurrences | Hash Map |
| Detect duplicates | Hash Set |
| Search sorted data | Binary Search |
| First/last duplicate position | Boundary Binary Search |
| Adjacent swaps | Bubble Sort |
| Insert into sorted prefix | Insertion Sort |
| Repeatedly select minimum | Selection Sort |
| Divide and merge | Merge Sort |
| Pivot and partition | Quick Sort |
| Pair in sorted array | Two Pointers |
| Unique values in sorted array | Slow/Fast Pointers |
| Fixed contiguous range | Fixed Sliding Window |
| Longest substring satisfying condition | Variable Sliding Window |
| Reverse node links | Three Pointers |
| Detect linked-list cycle | Floyd Slow/Fast |
| Merge sorted lists | Dummy Node + Two Pointers |
| Matching nested brackets | Stack |
| Next larger value to right | Monotonic Stack |
| FIFO operations | Queue |
| Reuse fixed array positions | Circular Queue + Modulo |
| Repeated smaller subproblem | Recursion / DP |
| Generate every choice | Backtracking |
| Visit tree nodes | DFS / BFS |

# 14. COMPLEXITY QUICK REFERENCE

| Algorithm / operation | Typical time | Typical extra space |
|---|---:|---:|
| Array index access | O(1) | O(1) |
| Linear search | O(n) | O(1) |
| Binary search | O(log n) | O(1) iterative |
| Hash lookup/insert | O(1) average | O(n) structure |
| Bubble/Selection/Insertion | O(n²) average | O(1) |
| Merge sort | O(n log n) | O(n) |
| Quick sort | O(n log n) average, O(n²) worst | O(log n) average stack |
| Stack push/pop | O(1) | O(n) storage |
| Queue enqueue/dequeue | O(1) suitable implementation | O(n) storage |
| Linked-list traversal | O(n) | O(1) iterative |
| Tree traversal | O(n) | O(h) DFS stack / O(w) BFS queue |
| Generate all subsets | O(n × 2ⁿ) | O(n × 2ⁿ) output |

`n` = input size; `k` = number of distinct characters/values; `h` = tree height; `w` = maximum tree width. State whether output space is included when explaining complexity.

# 15. FINAL PRACTICE CHECKLIST

## Arrays and strings
- [ ] Two Sum: brute force and Hash Map.
- [ ] Move Zeroes: in-place without changing order.
- [ ] Maximum Subarray: Kadane, including all-negative input.
- [ ] Missing Number: sum and XOR.
- [ ] Reverse String: two pointers.
- [ ] Valid Palindrome: ignore punctuation and case.
- [ ] Anagram: sorting and frequency count.
- [ ] First Non-Repeating Character: two passes.

## Hashing, searching, sorting and pointers
- [ ] Frequency Count and Duplicate Elements.
- [ ] Iterative Binary Search without off-by-one errors.
- [ ] First and Last Position using two boundary searches.
- [ ] Explain all five sorting algorithms and complexities.
- [ ] Pair Sum on sorted input.
- [ ] Remove Duplicates in-place.

## Sliding window and data structures
- [ ] Fixed-size maximum-sum window.
- [ ] Longest substring without repeating characters.
- [ ] Reverse linked list iteratively.
- [ ] Detect cycle with Floyd’s algorithm.
- [ ] Merge two sorted linked lists.
- [ ] Valid Parentheses using stack.
- [ ] Next Greater Element using monotonic stack.
- [ ] Implement normal and circular queues.

## Recursion and trees
- [ ] Explain base case and recursive case.
- [ ] Compare naive, memoized, and iterative Fibonacci.
- [ ] Generate subsets using backtracking.
- [ ] Write inorder, preorder, and postorder.
- [ ] Write level-order traversal using a queue.

## How to explain any coding answer
1. Restate the problem.
2. Confirm assumptions and constraints.
3. Explain the simple approach.
4. Explain the optimization and chosen data structure.
5. Dry-run a small example.
6. Write clean code.
7. Test edge cases.
8. State time and auxiliary-space complexity.

**Final reminder:** Do not memorize code line by line. Learn the pattern, pointer movement, data-structure choice, and why the algorithm works. That makes it possible to adapt when the interviewer changes the input or constraints.
