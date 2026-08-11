# Divide and Conquer Algorithms

## 1. What Is This?

Divide and Conquer is a fundamental algorithm design paradigm. Instead of solving a large problem all at once, Divide and Conquer breaks the problem down into smaller sub-problems of the same type, solves them recursively, and then combines their solutions to form the final answer.

This module focuses on **Merge Sort**—a classical Divide and Conquer sorting algorithm that guarantees $O(N \log N)$ worst-case performance.

---

## 2. Core Idea

Every Divide and Conquer algorithm follows three distinct phases:

1. **Divide:** Split the problem into $K$ smaller, non-overlapping sub-problems (usually two equal halves $N/2$).
2. **Conquer:** Solve the sub-problems recursively. If sub-problems are small enough (base case, e.g. array size 1), solve them directly.
3. **Combine:** Merge the sub-problem solutions together to construct the solution to the original problem.

```text
                       [ 6 , 3 , 9 , 5 , 2 , 8 ]         <-- Original (Size N)
                                 │
                     ┌───────────┴───────────┐           <-- DIVIDE
                     ▼                       ▼
               [ 6 , 3 , 9 ]           [ 5 , 2 , 8 ]     (Halves N/2)
                 │       │               │       │
              ┌──┴──┐    │            ┌──┴──┐    │
              ▼     ▼    ▼            ▼     ▼    ▼
             [6]   [3]  [9]          [5]   [2]  [8]      <-- Base Cases (Size 1)
              └──┬──┘    │            └──┬──┘    │
                 ▼       │               ▼       │
               [3,6]     │             [2,5]     │       <-- CONQUER & COMBINE
                 └───┬───┘               └───┬───┘
                     ▼                       ▼
               [ 3 , 6 , 9 ]           [ 2 , 5 , 8 ]
                     └───────────┬───────────┘
                                 ▼
                       [ 2 , 3 , 5 , 6 , 8 , 9 ]         <-- Final Sorted Array!
```

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Divide** | Partitioning a problem into smaller sub-problems. |
| **Conquer** | Solving smaller sub-problems recursively. |
| **Combine / Merge** | Merging two already-sorted arrays into a single combined sorted array. |
| **Recurrence Relation** | A mathematical equation representing time complexity in terms of smaller input sizes ($T(n) = 2T(n/2) + O(n)$). |
| **Depth of Recursion Tree** | Total height levels of recursive splits $= \log_2 N$. |

---

## 4. Visual / Mental Model

### Two-Pointer Merge Phase Mechanics

Merging sorted left subarray `[3, 6, 9]` and sorted right subarray `[2, 5, 8]`:

```text
  Left Subarray:  [ 3 , 6 , 9 ]       Right Subarray: [ 2 , 5 , 8 ]
                    i                                   j
  Target Temp:    [   ,   ,   ,   ,   ,   ]
                    k

  Compare Left[i=0] (3) vs Right[j=0] (2):
  Right is smaller (2 < 3)! Copy Right[j] to Temp[k]. Increment j++, k++.

  Temp Array:     [ 2 ,   ,   ,   ,   ,   ]
                        k
```

Repeat until all elements are copied into `temp`, then overwrite back into original array!

---

## 5. Operations / Techniques

### 1. Midpoint Calculation
To split array cleanly without integer overflow:
$$\text{mid} = \text{si} + \frac{\text{ei} - \text{si}}{2}$$

### 2. Recursive Splitting
- Left half call: `mergeSort(arr, si, mid)`
- Right half call: `mergeSort(arr, mid + 1, ei)`

### 3. Two-Pointer Merging
- Iterate pointer $i$ from `si` to `mid` and pointer $j$ from `mid+1` to `ei`.
- Append smaller element to `temp[k]`.
- Copy remaining unexhausted elements from left or right subarray.

---

## 6. Worked Examples

### Worked Example: Merge Sort Step Trace

**Input Array:** `[4, 1, 3, 2]`

- **Step 1 (Divide):**
  - Split into `[4, 1]` and `[3, 2]`.
  - Split `[4, 1]` into `[4]` and `[1]`.
- **Step 2 (Merge Left):**
  - Merge `[4]` and `[1]` $\implies$ Compare $4 > 1 \implies$ `[1, 4]`.
- **Step 3 (Divide Right):**
  - Split `[3, 2]` into `[3]` and `[2]`.
- **Step 4 (Merge Right):**
  - Merge `[3]` and `[2]` $\implies$ Compare $3 > 2 \implies$ `[2, 3]`.
- **Step 5 (Final Merge):**
  - Merge `[1, 4]` and `[2, 3]`:
    - Compare $1 < 2 \implies$ pick `1`.
    - Compare $4 > 2 \implies$ pick `2`.
    - Compare $4 > 3 \implies$ pick `3`.
    - Copy remaining `4`.
- **Result:** **`[1, 2, 3, 4]`**

---

## 7. Java Implementation Concepts

- **Auxiliary Array Allocation:** Merge Sort requires an extra temporary array `temp` of size `ei - si + 1` during the merge phase.
- **In-Place Limitations:** Classic Merge Sort is NOT in-place because it requires $O(N)$ auxiliary space for merging.

---

## 8. Problem-Solving Patterns

### Pattern 1: Divide and Conquer Recurrence
- **When to think of it:** Sorting, counting inversions in an array, matrix multiplication (Strassen's), finding median of two sorted arrays.
- **Mental Approach:** Partition problem at `mid`, recurse on both halves, merge results in $O(N)$ time.

---

## 9. Algorithms

### Merge Sort Algorithm
- **Problem solved:** Sort array of size $N$ in guaranteed $O(N \log N)$ time.
- **Pseudocode:**
  ```text
  function mergeSort(arr, si, ei):
      if si >= ei return // Base case (size 1)
      mid = si + (ei - si) / 2
      mergeSort(arr, si, mid)      // Sort Left Half
      mergeSort(arr, mid + 1, ei)  // Sort Right Half
      merge(arr, si, mid, ei)      // Merge Both Halves

  function merge(arr, si, mid, ei):
      temp = new int[ei - si + 1]
      i = si, j = mid + 1, k = 0
      while i <= mid and j <= ei:
          if arr[i] <= arr[j]: temp[k++] = arr[i++]
          else: temp[k++] = arr[j++]
      while i <= mid: temp[k++] = arr[i++]
      while j <= ei:  temp[k++] = arr[j++]
      for idx = 0 to temp.length - 1:
          arr[si + idx] = temp[idx]
  ```
- **Time Complexity:** $O(N \log N)$ All cases (Best, Average, Worst) | **Space:** $O(N)$

---

## 10. Complexity Reference

| Case | Time Complexity | Derivation / Reason |
|---|---|---|
| **Best Case** | $O(N \log N)$ | Always splits in half and merges. |
| **Average Case** | $O(N \log N)$ | Balanced recursion tree depth $\log_2 N$. |
| **Worst Case** | $O(N \log N)$ | Guarantees $O(N \log N)$ even on reverse sorted data! |
| **Space Complexity** | $O(N)$ | Temporary array during merge phase + $O(\log N)$ call stack. |
| **Stability** | **Stable** | Preserves order of equal elements (`arr[i] <= arr[j]`). |

---

## 11. Common Mistakes

- **Incorrect Loop Boundaries in Merge:** Writing `i < mid` instead of `i <= mid` or `j < ei` instead of `j <= ei`, dropping the boundary elements.
- **Incorrect Copying Back to Original Array:** Copying `temp[idx]` into `arr[idx]` instead of `arr[si + idx]` (overwriting start of array instead of target range!).
- **Midpoint Integer Overflow:** Writing `(si + ei) / 2` instead of `si + (ei - si) / 2`.

---

## 12. Edge Cases

- **Single Element Array (`si == ei`):** Base case triggers immediately.
- **Two Element Array (`si + 1 == ei`).**
- **Already Sorted Array:** Still takes $O(N \log N)$ time (unlike Quick Sort or Bubble Sort).
- **Array with All Duplicate Elements.**

---

## 13. Interview Questions

### Beginner
1. What are the three main steps of the Divide and Conquer paradigm?
2. Why is Merge Sort guaranteed to run in $O(N \log N)$ time even in the worst case?

### Intermediate
1. Explain why Merge Sort requires $O(N)$ auxiliary space complexity.
2. Is Merge Sort a stable sorting algorithm? Explain why `arr[i] <= arr[j]` guarantees stability.

### Advanced
1. Derive the time complexity of Merge Sort using the Master Theorem ($T(N) = 2T(N/2) + O(N)$).
2. Compare Merge Sort and Quick Sort regarding worst-case time complexity, space complexity, and cache friendliness.

---

## 14. Real-World Applications

- **External Sorting:** Sorting gigantic datasets (terabytes) stored on disk that cannot fit into RAM (used in database engines and Hadoop/Spark MapReduce).
- **Linked List Sorting:** Standard library default for sorting Linked Lists because linked lists allow $O(1)$ pointer merging without auxiliary memory allocation!
- **Counting Inversions:** Calculating how close an array is to being completely sorted.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`MergeSort.java`](MergeSort.java) | Complete Merge Sort implementation with recursive partitioning and two-pointer array merging. |

---

## 16. Related Topics

### Prerequisites
- Recursion Basics & Call Stack.
- 1D Arrays & Two Pointers.

### Related Topics
- Quick Sort & Selection Algorithms.
- Binary Search (Divide and Conquer search).

### Next Topics
- Backtracking (Search space exploration with undoing choices).
- Linked List Sorting.

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for Merge Sort recursive tree splitting and array merging here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
