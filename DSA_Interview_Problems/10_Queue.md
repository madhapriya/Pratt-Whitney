# Queue – DSA Interview Practice

**Problems:** 2  
**Languages:** Java and Python

---

# 1. Implement Queue

**Difficulty:** Easy  
**Pattern:** Queue / FIFO

## Problem Statement

Implement FIFO queue operations: enqueue, dequeue, peek/front, and isEmpty.

## Input Format

```text
enqueue(10), enqueue(20), dequeue(), peek()
```

## Output Format

```text
dequeue returns 10; peek returns 20
```

## Constraints

- Handle empty dequeue/peek; define capacity if using a fixed-size array.

## Example

```text
Input:  enqueue(10), enqueue(20), dequeue(), peek()
Output: dequeue returns 10; peek returns 20

Additional Example 2 Input:  enqueue(1), enqueue(2), dequeue()
Additional Example 2 Output: dequeue returns 1; front=2

Additional Example 3 Input:  enqueue(7), peek(), dequeue()
Additional Example 3 Output: peek=7; dequeue=7
```

## Approach

Use a linked list with `front` and `rear` pointers. Enqueue attaches a new node at the rear. Dequeue removes the front node. Update both pointers when the queue becomes empty. These operations take constant time.

## Iteration / Dry Run

1. Enqueue 10: front=rear=node10.
2. Enqueue 20: node10.next=node20 and rear=node20.
3. Dequeue returns 10 and moves front to node20.
4. Peek returns 20.
5. Removing the last node sets both front and rear to null.

## Java Solution

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

## Python Solution

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

**Time:** `O(1) per enqueue, dequeue, and peek`  
**Space:** `O(n) for n stored elements`

---

# 2. Circular Queue

**Difficulty:** Medium  
**Pattern:** Queue / Circular Array

## Problem Statement

Implement a fixed-capacity queue that reuses array positions by wrapping indices around.

## Input Format

```text
capacity=3; enqueue 1,2,3; dequeue; enqueue 4
```

## Output Format

```text
front=2, rear=4; queue is full
```

## Constraints

- Capacity is positive; enqueue fails when full and dequeue fails when empty.

## Example

```text
Input:  capacity=3; enqueue 1,2,3; dequeue; enqueue 4
Output: front=2, rear=4; queue is full

Additional Example 2 Input:  capacity=2; enQueue(1), enQueue(2), deQueue(), enQueue(3)
Additional Example 2 Output: front=2, rear=3; full

Additional Example 3 Input:  capacity=1; enQueue(9), enQueue(8), deQueue()
Additional Example 3 Output: true, false, true
```

## Approach

Store elements in a fixed-size array and maintain `front`, `rear`, and `size`. Advance an index using `(index + 1) % capacity` so freed array positions can be reused. The size distinguishes full from empty.

## Iteration / Dry Run

Capacity=3; initially front=0,rear=-1,size=0.
1. Enqueue 10: rear=(−1+1)%3=0; size=1.
2. Enqueue 20: rear=1; size=2.
3. Enqueue 30: rear=2; size=3 (full).
4. Dequeue: front=(0+1)%3=1; size=2.
5. Enqueue 40: rear=(2+1)%3=0; store at index 0; size=3.
6. Front=20, rear=40; wraparound works.

## Java Solution

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

## Python Solution

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

**Time:** `O(1) per operation`  
**Space:** `O(k), where k is the queue capacity`

---
