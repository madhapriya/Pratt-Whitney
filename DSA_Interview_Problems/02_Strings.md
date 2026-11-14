# Strings – DSA Interview Practice

**Problems:** 4  
**Languages:** Java and Python

---

# 1. Reverse String

**Difficulty:** Easy  
**Pattern:** String / Two Pointers

## Problem Statement

Reverse the given character array in place.

## Input Format

```text
s = ['h','e','l','l','o']
```

## Output Format

```text
['o','l','l','e','h']
```

## Constraints

- 1 <= s.length <= 10^5; modify the input array rather than returning a new one.

## Example

```text
Input:  s = ['h','e','l','l','o']
Output: ['o','l','l','e','h']

Additional Example 2 Input:  s=['H','a','n','n','a','h']
Additional Example 2 Output: ['h','a','n','n','a','H']

Additional Example 3 Input:  s=['A']
Additional Example 3 Output: ['A']
```

## Approach

Place one pointer at the beginning and another at the end of the character array. Swap those characters, then move both pointers inward. Stop when the pointers meet or cross.

## Iteration / Dry Run

1. left=0,right=4: swap h and o → [o,e,l,l,h].
2. left=1,right=3: swap e and l → [o,l,l,e,h].
3. left=2,right=2: stop. Result=[o,l,l,e,h].

## Java Solution

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

## Python Solution

```python
def reverse_string(s):
    l, r = 0, len(s) - 1
    while l < r:
        s[l], s[r] = s[r], s[l]
        l += 1
        r -= 1
```

**Time:** `O(n)`  
**Space:** `O(1)`

---

# 2. Valid Palindrome

**Difficulty:** Easy  
**Pattern:** String / Two Pointers

## Problem Statement

Return true if the string reads the same forward and backward after ignoring non-alphanumeric characters and letter case.

## Input Format

```text
s = 'A man, a plan, a canal: Panama'
```

## Output Format

```text
true
```

## Constraints

- 1 <= s.length <= 2 * 10^5; compare letters/digits case-insensitively.

## Example

```text
Input:  s = 'A man, a plan, a canal: Panama'
Output: true

Additional Example 2 Input:  s='race a car'
Additional Example 2 Output: false

Additional Example 3 Input:  s=' '
Additional Example 3 Output: true
```

## Approach

Move two pointers inward from the start and end. Skip every character that is not a letter or digit. Compare the remaining characters without considering case. Any mismatch means the string is not a palindrome.

## Iteration / Dry Run

1. Compare the first and last meaningful characters A/a: equal.
2. Skip spaces and punctuation.
3. Compare each next pair inward; every pair matches.
4. Pointers meet, so return true.

## Java Solution

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

## Python Solution

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

**Time:** `O(n)`  
**Space:** `O(1)`

---

# 3. Valid Anagram

**Difficulty:** Easy  
**Pattern:** String / Frequency Counting

## Problem Statement

Return true if strings s and t contain exactly the same characters with the same frequencies.

## Input Format

```text
s = 'anagram', t = 'nagaram'
```

## Output Format

```text
true
```

## Constraints

- s and t contain lowercase English letters in the fixed-array version; lengths can be up to 5 * 10^4.

## Example

```text
Input:  s = 'anagram', t = 'nagaram'
Output: true

Additional Example 2 Input:  s='rat', t='car'
Additional Example 2 Output: false

Additional Example 3 Input:  s='listen', t='silent'
Additional Example 3 Output: true
```

## Approach

Use a frequency array for the 26 lowercase English letters. Increment counts for the first string and decrement counts for the second string. If every count is zero, both strings contain exactly the same characters with the same frequencies.

## Iteration / Dry Run

1. Start 26 counters at zero.
2. For every index, increment the character from s and decrement the character from t.
3. Since the strings have the same counts, every counter returns to zero.
4. Return true.

## Java Solution

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

## Python Solution

```python
def is_anagram(s, t):
    if len(s) != len(t): return False
    count = [0] * 26
    for a, b in zip(s, t):
        count[ord(a)-97] += 1
        count[ord(b)-97] -= 1
    return all(x == 0 for x in count)
```

**Time:** `O(n)`  
**Space:** `O(1) for a fixed 26-letter alphabet`

---

# 4. First Non-Repeating Character

**Difficulty:** Easy  
**Pattern:** String / Frequency Map

## Problem Statement

Find the first character in a string whose frequency is exactly one. Return the required sentinel if none exists.

## Input Format

```text
s = 'swiss'
```

## Output Format

```text
'w'
```

## Constraints

- 1 <= s.length <= 10^5; clarify the expected return type/sentinel with the platform.

## Example

```text
Input:  s = 'swiss'
Output: 'w'

Additional Example 2 Input:  s='leetcode'
Additional Example 2 Output: 'l'

Additional Example 3 Input:  s='aabb'
Additional Example 3 Output: '$'
```

## Approach

Count the frequency of every character in a first pass. In a second pass, scan the original string from left to right and return the first character whose frequency is one. This preserves the original character order.

## Iteration / Dry Run

1. First pass builds counts, including s=3 and w=1.
2. Second pass preserves original order: s is skipped because count=3.
3. w has count=1; return 'w'.

## Java Solution

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

## Python Solution

```python
from collections import Counter
def first_unique(s):
    freq = Counter(s)
    for c in s:
        if freq[c] == 1:
            return c
    return '$'
```

**Time:** `O(n) average`  
**Space:** `O(k), where k is the number of distinct characters`

---
