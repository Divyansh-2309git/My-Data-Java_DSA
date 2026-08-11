# Sorting Algorithms

## 1. What Is This?

Sorting is the process of arranging elements in an array or collection in a specific order (typically ascending or descending).

Sorting is one of the most fundamental operations in computer science because ordered data enables fast $O(\log N)$ binary searching, efficient grouping, duplicate detection, and optimal greedy decision-making.

---

## 2. Core Idea

Sorting algorithms are broadly categorized into:

1. **Comparison-Based Sorting:** Compares elements using operators (`<`, `>`). Lower bound complexity for comparison sorting is $O(N \log N)$. Examples: Bubble Sort, Selection Sort, Insertion Sort, Merge Sort, Quick Sort.
2. **Non-Comparison Sorting:** Uses element properties (e.g. integer range, digit frequencies) without comparing pairs directly. Can achieve $O(N)$ linear time! Example: Counting Sort.

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **In-Place Sorting** | An algorithm that sorts the array using $O(1)$ auxiliary memory without creating array copies. |
| **Stability** | A sorting algorithm is **stable** if it preserves the relative order of duplicate elements. |
| **Pass** | A complete traversal over the unsorted section of the array. |
| **Swapping** | Exchanging the positions of two elements in memory using a temporary variable. |
| **Frequency Array** | An auxiliary array used in Counting Sort to count occurrences of each distinct integer value. |

---

## 4. Visual / Mental Model

### Bubble Sort Mental Model (Bubbling Max to Right)
In each pass, adjacent elements are compared. Larger element "bubbles up" to the right end:

```text
Pass 1:  [ 5, 4, 1, 3, 2 ] ──► Compare (5,4) swap ──► [ 4, 5, 1, 3, 2 ]
                           ──► Compare (5,1) swap ──► [ 4, 1, 5, 3, 2 ]
                           ──► Compare (5,3) swap ──► [ 4, 1, 3, 5, 2 ]
                           ──► Compare (5,2) swap ──► [ 4, 1, 3, 2, | 5 ]  (5 is in place!)
```

### Selection Sort Mental Model (Pick Minimum)
In each pass, find the global minimum in the unsorted portion and swap it to the front:

```text
Unsorted: [ 5, 4, 1, 3, 2 ] ──► Min is 1 at idx 2 ──► Swap with idx 0 ──► [ 1 | 4, 5, 3, 2 ]
Unsorted: [ 4, 5, 3, 2 ]    ──► Min is 2 at idx 4 ──► Swap with idx 1 ──► [ 1, 2 | 5, 3, 4 ]
```

---

## 5. Operations / Techniques

### 1. Bubble Sort
- Compares adjacent elements `arr[j]` and `arr[j+1]`. Swaps if out of order.
- After $i$ passes, the largest $i$ elements are placed at the correct rightmost positions.
- **Optimization:** Add a boolean `swapped` flag. If no swaps occur during a pass, the array is already sorted—break early in $O(N)$ time!

### 2. Selection Sort
- Selects the smallest element from the unsorted subarray and swaps it with the first unsorted index.
- Minimizes memory writes ($O(N)$ total swaps), making it useful when writing to memory is expensive.

### 3. Counting Sort
- Best used when numbers are in a small, known integer range $[0 \dots K]$.
- Counts frequencies of each element, then overwrites original array by repeating each number according to its frequency count.

### 4. Inbuilt Sorting (`Arrays.sort()`)
- Java uses **Dual-Pivot Quicksort** for primitive types ($O(N \log N)$ average) and **Timsort** (hybrid Merge/Insertion sort) for Object arrays.

---

## 6. Worked Examples

### Worked Example: Counting Sort

**Input Array:** `[1, 4, 1, 3, 2, 4, 3, 7]`

- **Step 1:** Find maximum value $\max = 7$.
- **Step 2:** Create frequency count array `count` of size $\max + 1 = 8$:
  ```text
  Index: 0  1  2  3  4  5  6  7
  Count: [0, 2, 1, 2, 2, 0, 0, 1]
  ```
- **Step 3:** Overwrite original array iterating through `count` array:
  - Value `1` appears `2` times $\implies$ write `1, 1`
  - Value `2` appears `1` time $\implies$ write `2`
  - Value `3` appears `2` times $\implies$ write `3, 3`
  - Value `4` appears `2` times $\implies$ write `4, 4`
  - Value `7` appears `1` time $\implies$ write `7`
- **Result:** `[1, 1, 2, 3, 3, 4, 4, 7]`

---

## 7. Java Implementation Concepts

- **Primitives vs Objects:**
  - `Arrays.sort(int[])`: Sorts primitive array in ascending order.
  - `Arrays.sort(Integer[], Collections.reverseOrder())`: Sorts object arrays in descending order using custom Comparator.
- **Custom Comparator / Lambda:**
  ```java
  Arrays.sort(arr, (a, b) -> b - a); // Descending sort for Objects
  ```

---

## 8. Problem-Solving Patterns

### Pattern 1: Frequency Array (Non-comparison Bucket/Counting)
- **When to think of it:** Numbers are integers within a small, non-negative range (e.g. ages, test scores $0..100$).
- **Mental Approach:** Count frequencies in $O(N)$ time, then fill array sequentially.

### Pattern 2: Minimum Swap Optimization
- **When to think of it:** Minimizing swap operations in hardware with slow write speeds (Flash memory/EEPROM). Selection Sort performs at most $N-1$ swaps.

---

## 9. Algorithms

### 1. Bubble Sort (With Early Stopping Optimization)
- **Pseudocode:**
  ```text
  function bubbleSort(arr):
      n = arr.length
      for i = 0 to n - 2:
          swapped = false
          for j = 0 to n - 2 - i:
              if arr[j] > arr[j+1]:
                  swap(arr[j], arr[j+1])
                  swapped = true
          if not swapped break
  ```
- **Time Complexity:** $O(N^2)$ Worst/Average, $O(N)$ Best (Optimized) | **Space:** $O(1)$

### 2. Selection Sort
- **Pseudocode:**
  ```text
  function selectionSort(arr):
      n = arr.length
      for i = 0 to n - 2:
          minIdx = i
          for j = i + 1 to n - 1:
              if arr[j] < arr[minIdx]: minIdx = j
          swap(arr[i], arr[minIdx])
  ```
- **Time Complexity:** $O(N^2)$ All cases | **Space:** $O(1)$

### 3. Counting Sort
- **Pseudocode:**
  ```text
  function countingSort(arr):
      largest = max(arr)
      count = new int[largest + 1]
      for num in arr: count[num]++
      j = 0
      for i = 0 to count.length - 1:
          while count[i] > 0:
              arr[j] = i
              j++
              count[i]--
  ```
- **Time Complexity:** $O(N + K)$ | **Space Complexity:** $O(K)$ where $K = \max(\text{arr})$

---

## 10. Complexity Reference

| Algorithm | Best Case | Average Case | Worst Case | Space Complexity | Stable? | In-Place? |
|---|---|---|---|---|---|---|
| **Bubble Sort** | $O(N)$ | $O(N^2)$ | $O(N^2)$ | $O(1)$ | Yes | Yes |
| **Selection Sort** | $O(N^2)$ | $O(N^2)$ | $O(N^2)$ | $O(1)$ | No | Yes |
| **Counting Sort** | $O(N + K)$ | $O(N + K)$ | $O(N + K)$ | $O(K)$ | Yes | No |
| **Inbuilt `Arrays.sort`** | $O(N \log N)$ | $O(N \log N)$ | $O(N \log N)$ | $O(\log N)$ | Depends (Primitives: No, Objects: Yes) | Yes |

---

## 11. Common Mistakes

- **Inner Loop Bound Errors in Bubble Sort:** Running inner loop up to `n-1` every pass instead of `n - 1 - i` (wasting comparisons on already-sorted right elements).
- **Out of Memory in Counting Sort:** Using Counting Sort when maximum value $K = 10^9$. Allocating `new int[10^9]` will crash Java with `OutOfMemoryError`. Use Counting Sort ONLY when $K$ is reasonably small ($K \approx 10^6$).
- **Confusing Selection Sort Min Index:** Updating `minIdx` inside the loop but swapping inside the inner loop instead of after the inner loop finishes.

---

## 12. Edge Cases

- **Already Sorted Array:** Bubble sort optimized finishes in 1 pass $O(N)$.
- **Reverse Sorted Array:** Triggers maximum possible swaps in Bubble Sort ($N(N-1)/2$).
- **Array with Duplicate Elements:** Stability testing.
- **Single Element / Empty Array.**

---

## 13. Interview Questions

### Beginner
1. What is the difference between an In-Place and Out-of-Place sorting algorithm?
2. Explain the concept of algorithm Stability with an example.

### Intermediate
1. Why does Selection Sort perform $O(N^2)$ comparisons even on an already sorted array?
2. How does the `swapped` flag optimize Bubble Sort best-case time complexity to $O(N)$?

### Advanced
1. Compare Dual-Pivot Quicksort and Timsort as implemented in Java's standard library.
2. Under what exact conditions is Counting Sort preferred over $O(N \log N)$ algorithms like Merge Sort or Quick Sort?

---

## 14. Real-World Applications

- **E-Commerce Product Filtering:** Sorting items by price, ratings, or popularity.
- **Database Indexing:** B-Trees and database query engines maintain sorted indices for rapid point and range queries.
- **Leaderboards & Gaming:** Ranking player scores dynamically.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`BubbleSort.java`](BubbleSort.java) | Standard and optimized Bubble Sort algorithm bubbling largest elements to the right. |
| [`SelectionSort.java`](SelectionSort.java) | Selection Sort algorithm finding global minimum in unsorted portion and swapping to front. |
| [`CountingSort.java`](CountingSort.java) | Non-comparison Counting Sort algorithm achieving $O(N+K)$ linear sorting time. |
| [`InbuiltSort.java`](InbuiltSort.java) | Java standard library `Arrays.sort()`, partial range sorting, and descending sorting using `Collections.reverseOrder()`. |

---

## 16. Related Topics

### Prerequisites
- 1D Arrays & Nested Loops.

### Related Topics
- Divide & Conquer (Merge Sort, Quick Sort).
- Priority Queues & Heap Sort.

### Next Topics
- Dynamic Arrays (`ArrayList`).
- Two-Pointer Searching & Optimization.

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for Bubble/Selection/Counting Sort step-by-step traces here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
