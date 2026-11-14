# Stack – DSA Interview Practice

**Problems:** 2  
**Languages:** Java and Python

---

# 1. Valid Parentheses

**Difficulty:** Easy  
**Pattern:** Stack / Matching Brackets

## Problem Statement

Determine whether every opening bracket has a matching closing bracket in the correct order.

## Input Format

```text
s = '({[]})'
```

## Output Format

```text
true
```

## Constraints

- 1 <= s.length <= 10^4; characters are only (), [], {}.

## Example

```text
Input:  s = '({[]})'
Output: true

Additional Example 2 Input:  s='([)]'
Additional Example 2 Output: false

Additional Example 3 Input:  s='((()))'
Additional Example 3 Output: true
```

## Approach

Use a stack to store opening brackets. When a closing bracket appears, the stack must contain its matching opening bracket at the top. Pop the match. The string is valid only when no mismatch occurs and the stack is empty at the end.

## Iteration / Dry Run

1. Read '('; push.
2. Read '{'; push.
3. Read '['; push.
4. Read ']'; top '[' matches, pop.
5. Read '}'; top '{' matches, pop.
6. Read ')'; top '(' matches, pop.
7. Stack is empty; return true.

## Java Solution

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

## Python Solution

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

**Time:** `O(n)`  
**Space:** `O(n)`

---

# 2. Next Greater Element

**Difficulty:** Medium  
**Pattern:** Stack / Monotonic Stack

## Problem Statement

For every array element, find the first strictly greater element to its right; use -1 if none exists.

## Input Format

```text
a = [4,5,2,10]
```

## Output Format

```text
[5,10,10,-1]
```

## Constraints

- 1 <= a.length <= 10^4; equal values are not greater.

## Example

```text
Input:  a = [4,5,2,10]
Output: [5,10,10,-1]

Additional Example 2 Input:  a=[2,2,3]
Additional Example 2 Output: [3,3,-1]

Additional Example 3 Input:  a=[5,4,3]
Additional Example 3 Output: [-1,-1,-1]
```

## Approach

Use a monotonic decreasing stack of indices. When the current value is greater than the value at the top index, pop that index and record the current value as its next greater element. Each index is pushed and popped at most once.

## Iteration / Dry Run

1. Read 4; push index 0.
2. Read 5; 5>4, pop index 0 and set answer[0]=5; push index 1.
3. Read 2; push index 2.
4. Read 10; pop index 2 and set 10, then pop index 1 and set 10; push index 3.
5. Index 3 remains -1.

## Java Solution

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

## Python Solution

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

**Time:** `O(n)`  
**Space:** `O(n)`

---
