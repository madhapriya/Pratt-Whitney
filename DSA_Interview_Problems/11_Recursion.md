# Recursion – DSA Interview Practice

**Problems:** 3  
**Languages:** Java and Python

---

# 1. Factorial

**Difficulty:** Easy  
**Pattern:** Recursion / Base Case

## Problem Statement

Return n factorial, the product of all positive integers from 1 through n; 0! is 1.

## Input Format

```text
n = 5
```

## Output Format

```text
120
```

## Constraints

- 0 <= n <= 20 for the Java `long` implementation; Python integers can represent larger values.

## Example

```text
Input:  n = 5
Output: 120

Additional Example 2 Input:  n=0
Additional Example 2 Output: 1

Additional Example 3 Input:  n=6
Additional Example 3 Output: 720
```

## Approach

Use the base case `factorial(0) = 1` (and `factorial(1) = 1`). For n greater than one, return `n * factorial(n - 1)`. Each recursive call moves toward the base case.

## Iteration / Dry Run

1. factorial(5) calls 5×factorial(4).
2. Calls continue through 4×factorial(3), 3×factorial(2), 2×factorial(1).
3. Base case factorial(1)=1.
4. Return values upward: 2, 6, 24, 120.

## Java Solution

```java
class Solution {
    public long factorial(int n) {
        if (n <= 1) return 1;
        return n * factorial(n - 1);
    }
}
```

## Python Solution

```python
def factorial(n):
    if n <= 1: return 1
    return n * factorial(n - 1)
```

**Time:** `O(n)`  
**Space:** `O(n) recursion stack`

---

# 2. Fibonacci

**Difficulty:** Easy  
**Pattern:** Recursion / Memoization

## Problem Statement

Return the nth Fibonacci number, where F(0)=0, F(1)=1, and F(n)=F(n-1)+F(n-2).

## Input Format

```text
n = 6
```

## Output Format

```text
8
```

## Constraints

- 0 <= n <= 46 for the Java `int` implementation; Python integers can represent larger values.

## Example

```text
Input:  n = 6
Output: 8

Additional Example 2 Input:  n=0
Additional Example 2 Output: 0

Additional Example 3 Input:  n=7
Additional Example 3 Output: 13
```

## Approach

Use recursion with memoization. Return n for the base cases 0 and 1. For every other value, compute `fib(n - 1) + fib(n - 2)`, store the result, and reuse it instead of recalculating overlapping subproblems.

## Iteration / Dry Run

1. Start with the base values F(0)=0 and F(1)=1.
2. Calculate F(2)=F(1)+F(0)=1 and store it.
3. Calculate F(3)=F(2)+F(1)=2 and store it.
4. Calculate F(4)=F(3)+F(2)=3 and store it.
5. Calculate F(5)=F(4)+F(3)=5 and store it.
6. Calculate F(6)=F(5)+F(4)=5+3=8.
7. Return 8.

## Java Solution

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

## Python Solution

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

**Time:** `O(n)`  
**Space:** `O(n) memoization and recursion stack`

---

# 3. Subsets

**Difficulty:** Medium  
**Pattern:** Recursion / Backtracking

## Problem Statement

Generate every possible subset of an input array of distinct values, including the empty set.

## Input Format

```text
nums = [1,2]
```

## Output Format

```text
[[],[1],[2],[1,2]]
```

## Constraints

- 0 <= nums.length <= 10; input values are distinct in this version.

## Example

```text
Input:  nums = [1,2]
Output: [[],[1],[2],[1,2]]

Additional Example 2 Input:  nums=[0]
Additional Example 2 Output: [[],[0]]

Additional Example 3 Input:  nums=[1,2,3]
Additional Example 3 Output: 8 subsets
```

## Approach

Use backtracking. At each array position, explore two choices: exclude the current value or include it. When every position has been considered, add a copy of the current selection to the result.

## Iteration / Dry Run

1. At value 1, explore exclude branch.
2. At value 2 in that branch, exclude → []; include → [2].
3. Backtrack and include value 1.
4. At value 2, exclude → [1]; include → [1,2].
5. Four leaves represent all subsets.

## Java Solution

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

## Python Solution

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

**Time:** `O(n × 2^n)`  
**Space:** `O(n × 2^n) for the output`

---
