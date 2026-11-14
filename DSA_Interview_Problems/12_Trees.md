# Trees – DSA Interview Practice

**Problems:** 4  
**Languages:** Java and Python

---

# 1. Inorder – Left, Root, Right

**Difficulty:** Easy  
**Pattern:** Tree / DFS Inorder

## Problem Statement

Return a binary tree's values in inorder: visit left subtree, node, then right subtree.

## Input Format

```text
Tree: root 1; left 2 with children 4,5; right 3
```

## Output Format

```text
[4,2,5,1,3]
```

## Constraints

- 0 <= number of nodes <= 100; node values may repeat.

## Example

```text
Input:  Tree: root 1; left 2 with children 4,5; right 3
Output: [4,2,5,1,3]

Additional Example 2 Input:  tree=[1,null,2,3]
Additional Example 2 Output: [1,3,2]

Additional Example 3 Input:  tree=[7]
Additional Example 3 Output: [7]
```

## Approach

Use recursive depth-first search. First traverse the left subtree, then visit the current node, and finally traverse the right subtree.

## Iteration / Dry Run

1. Start at root 1 and move to its left child 2.
2. Move left again to node 4. Node 4 has no left child, so visit 4.
3. Return to node 2 and visit 2.
4. Traverse node 2's right child and visit 5.
5. Return to root and visit 1.
6. Traverse the root's right child and visit 3.
7. Final order: [4,2,5,1,3].

## Java Solution

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

## Python Solution

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

**Time:** `O(n)`  
**Space:** `O(h) recursion stack`

---

# 2. Preorder – Root, Left, Right

**Difficulty:** Easy  
**Pattern:** Tree / DFS Preorder

## Problem Statement

Return a binary tree's values in preorder: visit node, left subtree, then right subtree.

## Input Format

```text
Tree: root 1; left 2 with children 4,5; right 3
```

## Output Format

```text
[1,2,4,5,3]
```

## Constraints

- 0 <= number of nodes <= 100; node values may repeat.

## Example

```text
Input:  Tree: root 1; left 2 with children 4,5; right 3
Output: [1,2,4,5,3]

Additional Example 2 Input:  tree=[1,null,2,3]
Additional Example 2 Output: [1,2,3]

Additional Example 3 Input:  tree=[7]
Additional Example 3 Output: [7]
```

## Approach

Use recursive depth-first search. Visit the current node first, then traverse the left subtree, followed by the right subtree.

## Iteration / Dry Run

1. Start at root 1 and visit it immediately.
2. Traverse the left child 2 and visit it.
3. Visit node 2's left child 4.
4. Visit node 2's right child 5.
5. Return to the root and visit its right child 3.
6. Final order: [1,2,4,5,3].

## Java Solution

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

## Python Solution

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

**Time:** `O(n)`  
**Space:** `O(h) recursion stack`

---

# 3. Postorder – Left, Right, Root

**Difficulty:** Easy  
**Pattern:** Tree / DFS Postorder

## Problem Statement

Return a binary tree's values in postorder: visit left subtree, right subtree, then node.

## Input Format

```text
Tree: root 1; left 2 with children 4,5; right 3
```

## Output Format

```text
[4,5,2,3,1]
```

## Constraints

- 0 <= number of nodes <= 100; node values may repeat.

## Example

```text
Input:  Tree: root 1; left 2 with children 4,5; right 3
Output: [4,5,2,3,1]

Additional Example 2 Input:  tree=[1,null,2,3]
Additional Example 2 Output: [3,2,1]

Additional Example 3 Input:  tree=[7]
Additional Example 3 Output: [7]
```

## Approach

Use recursive depth-first search. Traverse the left subtree, then the right subtree, and visit the current node only after both subtrees have been completed.

## Iteration / Dry Run

1. Start at root 1 and traverse its left subtree rooted at 2.
2. Visit node 2's left child 4.
3. Visit node 2's right child 5.
4. Visit node 2 only after both children; output so far is [4,5,2].
5. Traverse and visit the root's right child 3.
6. Visit root 1 last.
7. Final order: [4,5,2,3,1].

## Java Solution

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

## Python Solution

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

**Time:** `O(n)`  
**Space:** `O(h) recursion stack`

---

# 4. Level Order – BFS with Queue

**Difficulty:** Medium  
**Pattern:** Tree / Breadth-First Search

## Problem Statement

Return the binary tree values level by level from top to bottom, left to right.

## Input Format

```text
Tree: root 1; left 2 with children 4,5; right 3
```

## Output Format

```text
[[1],[2,3],[4,5]]
```

## Constraints

- 0 <= number of nodes <= 2000; empty tree returns an empty list.

## Example

```text
Input:  Tree: root 1; left 2 with children 4,5; right 3
Output: [[1],[2,3],[4,5]]

Additional Example 2 Input:  tree=[1,null,2,3]
Additional Example 2 Output: [[1],[2],[3]]

Additional Example 3 Input:  tree=[7]
Additional Example 3 Output: [[7]]
```

## Approach

Use a queue for breadth-first search. Process all nodes currently in the queue as one level. For each removed node, record its value and enqueue its non-null children. Repeat until the queue is empty.

## Iteration / Dry Run

1. Enqueue root 1. The queue is [1].
2. Level 1: remove 1 and record [1]. Enqueue its children 2 and 3.
3. Level 2: remove 2 and enqueue its children 4 and 5; then remove 3. Record [2,3].
4. Level 3: remove 4 and then 5. Record [4,5].
5. The queue is empty. Final result: [[1],[2,3],[4,5]].

## Java Solution

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

## Python Solution

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

**Time:** `O(n)`  
**Space:** `O(w), where w is the maximum tree width`

---
