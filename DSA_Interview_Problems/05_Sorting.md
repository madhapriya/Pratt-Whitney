# Sorting – DSA Interview Practice

**Problems:** 5  
**Languages:** Java and Python

---

# 1. Bubble Sort

**Difficulty:** Easy  
**Pattern:** Sorting / Adjacent Swaps

## Problem Statement

Sort an integer array in ascending order by repeatedly swapping adjacent out-of-order elements.

## Input Format

```text
a = [5,1,4,2,8]
```

## Output Format

```text
[1,2,4,5,8]
```

## Constraints

- Array length may be 0 or greater; values may be negative or repeated.

## Example

```text
Input:  a = [5,1,4,2,8]
Output: [1,2,4,5,8]

Additional Example 2 Input:  a=[3,2,1]
Additional Example 2 Output: [1,2,3]

Additional Example 3 Input:  a=[1,2,3]
Additional Example 3 Output: [1,2,3]
```

## Approach

Compare adjacent elements and swap them when they are in the wrong order. After each complete pass, the largest value in the unsorted portion moves to the end. Stop early if a pass performs no swaps.

## Iteration / Dry Run

Start [5,1,4,2,8].
Pass 1: swap 5/1 → [1,5,4,2,8]; swap 5/4 → [1,4,5,2,8]; swap 5/2 → [1,4,2,5,8]; 5/8 stays.
Pass 2: 1/4 stays; swap 4/2 → [1,2,4,5,8]; remaining pairs stay. Pass 3 has no swaps, so stop. Final [1,2,4,5,8].

## Java Solution

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

## Python Solution

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

**Time:** `O(n²) average/worst; O(n) best with early stop`  
**Space:** `O(1)`

---

# 2. Selection Sort

**Difficulty:** Easy  
**Pattern:** Sorting / Minimum Selection

## Problem Statement

Sort an integer array by repeatedly selecting the minimum value from the unsorted suffix.

## Input Format

```text
a = [64,25,12,22,11]
```

## Output Format

```text
[11,12,22,25,64]
```

## Constraints

- Array length may be 0 or greater; values may repeat.

## Example

```text
Input:  a = [64,25,12,22,11]
Output: [11,12,22,25,64]

Additional Example 2 Input:  a=[3,1,2]
Additional Example 2 Output: [1,2,3]

Additional Example 3 Input:  a=[5,5,2]
Additional Example 3 Output: [2,5,5]
```

## Approach

For each position, scan the remaining unsorted portion to find its minimum value. Swap that minimum into the current position. The sorted prefix grows by one after every pass.

## Iteration / Dry Run

Start [64,25,12,22,11]. Pass 1 minimum is 11; swap with 64 → [11,25,12,22,64]. Pass 2 minimum of suffix is 12; swap with 25 → [11,12,25,22,64]. Pass 3 minimum is 22; swap → [11,12,22,25,64]. Pass 4 is already ordered. Final [11,12,22,25,64].

## Java Solution

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

## Python Solution

```python
def selection_sort(a):
    for i in range(len(a) - 1):
        m = i
        for j in range(i + 1, len(a)):
            if a[j] < a[m]: m = j
        a[i], a[m] = a[m], a[i]
```

**Time:** `O(n²)`  
**Space:** `O(1)`

---

# 3. Insertion Sort

**Difficulty:** Easy  
**Pattern:** Sorting / Sorted Prefix

## Problem Statement

Sort an array by inserting each next element into its correct position in the already-sorted prefix.

## Input Format

```text
a = [5,2,4,6,1,3]
```

## Output Format

```text
[1,2,3,4,5,6]
```

## Constraints

- Array length may be 0 or greater; works especially well for small or nearly sorted inputs.

## Example

```text
Input:  a = [5,2,4,6,1,3]
Output: [1,2,3,4,5,6]

Additional Example 2 Input:  a=[3,1,2]
Additional Example 2 Output: [1,2,3]

Additional Example 3 Input:  a=[1,2,3]
Additional Example 3 Output: [1,2,3]
```

## Approach

Treat the first element as a sorted portion. For each next element, shift all larger elements in the sorted prefix one position to the right, then insert the current value into the gap.

## Iteration / Dry Run

Start [5,2,4,6,1,3]. Insert 2: shift 5 → [2,5,4,6,1,3]. Insert 4: shift 5 → [2,4,5,6,1,3]. Insert 6: no shift. Insert 1: shift 6,5,4,2 → [1,2,4,5,6,3]. Insert 3: shift 6,5,4 → [1,2,3,4,5,6].

## Java Solution

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

## Python Solution

```python
def insertion_sort(a):
    for i in range(1, len(a)):
        key, j = a[i], i - 1
        while j >= 0 and a[j] > key:
            a[j + 1] = a[j]
            j -= 1
        a[j + 1] = key
```

**Time:** `O(n²) average/worst; O(n) best`  
**Space:** `O(1)`

---

# 4. Merge Sort

**Difficulty:** Medium  
**Pattern:** Sorting / Divide and Conquer

## Problem Statement

Sort an array using divide and conquer: recursively sort halves, then merge the sorted halves.

## Input Format

```text
a = [38,27,43,3,9,82,10]
```

## Output Format

```text
[3,9,10,27,38,43,82]
```

## Constraints

- Array length may be 0 or greater; preserve duplicates.

## Example

```text
Input:  a = [38,27,43,3,9,82,10]
Output: [3,9,10,27,38,43,82]

Additional Example 2 Input:  a=[4,1,3,2]
Additional Example 2 Output: [1,2,3,4]

Additional Example 3 Input:  a=[5,-1,5,0]
Additional Example 3 Output: [-1,0,5,5]
```

## Approach

Repeatedly divide the array into two halves until each part contains one element. Merge adjacent sorted parts by repeatedly taking the smaller front element. Continue merging until one sorted array remains.

## Iteration / Dry Run

Start [38,27,43,3]. Split into [38,27] and [43,3], then into single elements [38],[27],[43],[3]. Merge first pair → [27,38]. Merge second pair → [3,43]. Final merge compares 27 with 3 (take 3), then 27 with 43 (take 27), then 38, then 43. Result [3,27,38,43].

## Java Solution

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

## Python Solution

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

**Time:** `O(n log n)`  
**Space:** `O(n)`

---

# 5. Quick Sort

**Difficulty:** Medium  
**Pattern:** Sorting / Pivot and Partition

## Problem Statement

Sort an array by selecting a pivot, partitioning values around it, and recursively sorting the partitions.

## Input Format

```text
a = [10,7,8,9,1,5]
```

## Output Format

```text
[1,5,7,8,9,10]
```

## Constraints

- Array length may be 0 or greater; pivot strategy affects performance.

## Example

```text
Input:  a = [10,7,8,9,1,5]
Output: [1,5,7,8,9,10]

Additional Example 2 Input:  a=[3,7,2,5]
Additional Example 2 Output: [2,3,5,7]

Additional Example 3 Input:  a=[4,4,1]
Additional Example 3 Output: [1,4,4]
```

## Approach

Choose a pivot and partition the array so values less than or equal to the pivot are placed before it and larger values after it. Recursively sort the two partitions.

## Iteration / Dry Run

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

## Java Solution

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

## Python Solution

```python
def quick_sort(a):
    if len(a) <= 1: return a
    pivot = a[-1]
    left = [x for x in a[:-1] if x <= pivot]
    right = [x for x in a[:-1] if x > pivot]
    return quick_sort(left) + [pivot] + quick_sort(right)
```

**Time:** `O(n log n) average; O(n²) worst`  
**Space:** `O(n) in the Python list-based version; O(log n) average Java recursion stack`

---
