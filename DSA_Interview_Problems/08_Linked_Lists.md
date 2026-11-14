# Linked Lists – DSA Interview Practice

**Problems:** 3  
**Languages:** Java and Python

---

# 1. Reverse Linked List

**Difficulty:** Easy  
**Pattern:** Linked List / Pointer Reversal

## Problem Statement

Reverse the next pointers of a singly linked list and return the new head.

## Input Format

```text
head = 1 → 2 → 3 → null
```

## Output Format

```text
3 → 2 → 1 → null
```

## Constraints

- 0 <= number of nodes <= 5000; node values may be negative.

## Example

```text
Input:  head = 1 → 2 → 3 → null
Output: 3 → 2 → 1 → null

Additional Example 2 Input:  head=1→2→null
Additional Example 2 Output: 2→1→null

Additional Example 3 Input:  head=null
Additional Example 3 Output: null
```

## Approach

Use three pointers: `previous`, `current`, and `next`. Save the next node before changing the current node's link. Reverse the link, then advance the pointers. When current becomes null, previous is the new head.

## Iteration / Dry Run

1. prev=null, cur=1; save next=2 and set 1.next=null.
2. Move prev=1, cur=2; save next=3 and set 2.next=1.
3. Move prev=2, cur=3; save next=null and set 3.next=2.
4. Stop and return prev (node 3).

## Java Solution

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

## Python Solution

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

**Time:** `O(n)`  
**Space:** `O(1)`

---

# 2. Detect Cycle

**Difficulty:** Easy  
**Pattern:** Linked List / Floyd's Algorithm

## Problem Statement

Determine whether a singly linked list contains a cycle.

## Input Format

```text
head = [3,2,0,-4], tail connects to index 1
```

## Output Format

```text
true
```

## Constraints

- 0 <= number of nodes <= 10^4; a cycle may connect any node back to an earlier node.

## Example

```text
Input:  head = [3,2,0,-4], tail connects to index 1
Output: true

Additional Example 2 Input:  1→2→3→null
Additional Example 2 Output: false

Additional Example 3 Input:  1→1 (self-loop)
Additional Example 3 Output: true
```

## Approach

Use Floyd's slow and fast pointer algorithm. Move slow by one link and fast by two links. If they meet, the list contains a cycle. If fast reaches null, there is no cycle.

## Iteration / Dry Run

1. Slow=fast=node 3.
2. First round: slow→2; fast→0.
3. Second round: slow→0; fast→-4→2.
4. Third round: slow→-4; fast→0→-4.
5. They meet; return true.

## Java Solution

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

## Python Solution

```python
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast: return True
    return False
```

**Time:** `O(n)`  
**Space:** `O(1)`

---

# 3. Merge Two Sorted Lists

**Difficulty:** Easy  
**Pattern:** Linked List / Two Pointers

## Problem Statement

Merge two sorted singly linked lists into one sorted linked list by reusing their nodes.

## Input Format

```text
list1 = 1 → 2 → 4; list2 = 1 → 3 → 4
```

## Output Format

```text
1 → 1 → 2 → 3 → 4 → 4
```

## Constraints

- Each list may be empty; values are sorted in non-decreasing order.

## Example

```text
Input:  list1 = 1 → 2 → 4; list2 = 1 → 3 → 4
Output: 1 → 1 → 2 → 3 → 4 → 4

Additional Example 2 Input:  list1=null, list2=0→2
Additional Example 2 Output: 0→2

Additional Example 3 Input:  list1=1→3, list2=2→4
Additional Example 3 Output: 1→2→3→4
```

## Approach

Use a dummy node and a tail pointer. Compare the current nodes from both sorted lists, attach the smaller node to the merged list, and advance that list. Finally, attach whichever list still has nodes.

## Iteration / Dry Run

1. Dummy tail starts before both lists.
2. Compare 1 and 1; attach first list's node 1.
3. Compare 2 and 1; attach second list's node 1.
4. Attach 2, then 3.
5. Attach one 4 and then the remaining 4. Return dummy.next.

## Java Solution

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

## Python Solution

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

**Time:** `O(m + n)`  
**Space:** `O(1) auxiliary space`

---
