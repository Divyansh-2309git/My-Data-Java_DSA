# 1D Arrays & Array Patterns

## 1. What Is This?

An Array is a fundamental linear data structure that stores a fixed-size sequence of elements of the same data type in contiguous memory locations. Because memory addresses are contiguous, arrays provide direct, random access to any element using its zero-based index in $O(1)$ constant time.

Arrays serve as the primary storage mechanism for many higher-level data structures (Stacks, Queues, Heaps, Hash Tables) and are central to essential problem-solving patterns like Two Pointers, Prefix Sums, Sliding Windows, and Subarray Optimizations (Kadane's Algorithm).

---

## 2. Core Idea

An array maps contiguous memory offsets directly to indices. The memory address of element at index $i$ is calculated as:

$$\text{Address}(i) = \text{Base Address} + (i \times \text{Size of Type})$$

```text
Index:        0      1      2      3      4
Value:     [ 10 ] [ 20 ] [ 30 ] [ 40 ] [ 50 ]
Address:   1000   1004   1008   1012   1016   (assuming 4-byte integers)
```

Because of this mathematical formula, looking up index `3` requires no searching—the computer computes memory address $1000 + 3 \times 4 = 1012$ instantly!

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Element** | An individual data item stored in an array. |
| **Index** | A zero-based integer representing the position of an element in the array ($0$ to $N-1$). |
| **Contiguous Memory** | Memory blocks allocated right next to each other without gaps. |
| **Subarray** | A contiguous slice/portion of an array. Total subarrays in array of size $N = \frac{N(N+1)}{2}$. |
| **Prefix Sum Array** | An auxiliary array where `prefix[i]` stores the sum of elements from index `0` to `i`. |
| **Kadane's Algorithm** | Dynamic programming / greedy algorithm to find the maximum sum contiguous subarray in $O(N)$ time. |
| **Two Pointers** | Pattern using two index markers (e.g., `start` and `end`) moving towards or away from each other. |

---

## 4. Visual / Mental Model

### Linear Memory & Subarrays vs Subsequences

For array `[1, 2, 3]`:
- **Subarrays (Contiguous):** `[1]`, `[1, 2]`, `[1, 2, 3]`, `[2]`, `[2, 3]`, `[3]`
- **Subsequence (Order maintained, non-contiguous allowed):** Includes `[1, 3]`
- **Subset (Any combination):** Includes `[]`, `[3, 1]`

```text
Original:   [ 10 , 20 , 30 , 40 ]
              │─────────│          --> Subarray [10, 20, 30] (VALID)
              │    │         │     --> Subsequence [10, 20, 40] (NOT a subarray!)
```

---

## 5. Operations / Techniques

| Operation | Explanation | Time Complexity | Space Complexity |
|---|---|---|---|
| **Access** | Retrieve element at index `i` (`arr[i]`) | $O(1)$ | $O(1)$ |
| **Update** | Modify element at index `i` (`arr[i] = val`) | $O(1)$ | $O(1)$ |
| **Linear Search** | Scan array from index `0` to `N-1` matching key | $O(N)$ | $O(1)$ |
| **Binary Search** | Divide sorted search space in half repeatedly | $O(\log N)$ | $O(1)$ |
| **Reverse Array** | Swap `arr[left]` and `arr[right]` while `left < right` | $O(N)$ | $O(1)$ |
| **Prefix Sum Query** | Range sum `[L..R]` via `prefix[R] - prefix[L-1]` | $O(1)$ post-prep | $O(N)$ prep space |
| **Kadane's Max Sum**| Track `currSum = max(val, currSum + val)` & `maxSum` | $O(N)$ | $O(1)$ |

---

## 6. Worked Examples

### Worked Example 1: Kadane's Algorithm for Max Subarray Sum

**Input Array:** `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`

- **Step 1:** Initialize `currSum = 0`, `maxSum = Integer.MIN_VALUE`.
- **Step 2:**
  - $i=0, v=-2 \implies \text{currSum} = -2 \implies \text{maxSum} = -2$. Reset $\text{currSum} = 0$.
  - $i=1, v=1 \implies \text{currSum} = 1 \implies \text{maxSum} = \max(-2, 1) = 1$.
  - $i=2, v=-3 \implies \text{currSum} = 1 + (-3) = -2 \implies \text{maxSum} = 1$. Reset $\text{currSum} = 0$.
  - $i=3, v=4 \implies \text{currSum} = 4 \implies \text{maxSum} = \max(1, 4) = 4$.
  - $i=4, v=-1 \implies \text{currSum} = 4 + (-1) = 3 \implies \text{maxSum} = 4$.
  - $i=5, v=2 \implies \text{currSum} = 3 + 2 = 5 \implies \text{maxSum} = \max(4, 5) = 5$.
  - $i=6, v=1 \implies \text{currSum} = 5 + 1 = 6 \implies \text{maxSum} = \max(5, 6) = 6$.
  - $i=7, v=-5 \implies \text{currSum} = 6 + (-5) = 1 \implies \text{maxSum} = 6$.
  - $i=8, v=4 \implies \text{currSum} = 1 + 4 = 5 \implies \text{maxSum} = 6$.
- **Result:** Maximum Subarray Sum is **`6`** (sub-array `[4, -1, 2, 1]`).

### Worked Example 2: Trapping Rainwater

**Heights Array:** `[4, 2, 0, 6, 3, 2, 5]`

- **Step 1:** Precompute `leftMax` array: `[4, 4, 4, 6, 6, 6, 6]`.
- **Step 2:** Precompute `rightMax` array: `[6, 6, 6, 6, 5, 5, 5]`.
- **Step 3:** For each index $i$, trapped water $= \min(\text{leftMax}[i], \text{rightMax}[i]) - \text{height}[i]$:
  - $i=0$: $\min(4, 6) - 4 = 0$
  - $i=1$: $\min(4, 6) - 2 = 2$
  - $i=2$: $\min(4, 6) - 0 = 4$
  - $i=3$: $\min(6, 6) - 6 = 0$
  - $i=4$: $\min(6, 5) - 3 = 2$
  - $i=5$: $\min(6, 5) - 2 = 3$
  - $i=6$: $\min(6, 5) - 5 = 0$
- **Total Trapped Water:** $0 + 2 + 4 + 0 + 2 + 3 + 0 = \mathbf{11}$.

---

## 7. Java Implementation Concepts

- **Array Declaration & Allocation:**
  ```java
  int[] arr = new int[5]; // Allocated with default zeros
  int[] numbers = {10, 20, 30, 40, 50}; // Literal initialization
  ```
- **Length Property:** `arr.length` is a field (not a method). Indices range from `0` to `arr.length - 1`.
- **Passing Arrays to Methods:** Arrays are passed by reference copy. Modifying array elements inside a method modifies the caller's array!
- **Utility Methods (`java.util.Arrays`):**
  - `Arrays.sort(arr)`: Dual-Pivot Quicksort $O(N \log N)$.
  - `Arrays.fill(arr, val)`: Fills array with initial value.
  - `Arrays.binarySearch(arr, key)`: Fast lookup on sorted arrays.

---

## 8. Problem-Solving Patterns

### Pattern 1: Two Pointers (Inward / Opposite direction)
- **When to think of it:** Reversing array, Pair sum in sorted array, Container with most water, Trapping rainwater.
- **Mental Approach:** Place `left = 0`, `right = n - 1`. Move markers inward based on sum/height comparison.

### Pattern 2: Prefix Sum Array
- **When to think of it:** Range sum queries `[L..R]`, subarray sums, finding equilibrium indices.
- **Mental Approach:** Build `prefix[i] = prefix[i-1] + arr[i]`. Subarray sum `sum(L, R) = prefix[R] - prefix[L-1]`.

### Pattern 3: Kadane's Accumulator
- **When to think of it:** Maximum/minimum contiguous subarray sum.
- **Mental Approach:** Maintain `currSum`. If `currSum < 0`, reset `currSum = 0` (or `max(arr[i], currSum + arr[i])`).

---

## 9. Algorithms

### 1. Binary Search
- **Problem solved:** Search key in sorted array in $O(\log N)$.
- **Pseudocode:**
  ```text
  function binarySearch(arr, key):
      start = 0, end = arr.length - 1
      while start <= end:
          mid = start + (end - start) / 2
          if arr[mid] == key return mid
          else if arr[mid] < key start = mid + 1
          else end = mid - 1
      return -1
  ```
- **Time Complexity:** $O(\log N)$ | **Space:** $O(1)$

### 2. Kadane's Algorithm
- **Problem solved:** Find maximum contiguous subarray sum.
- **Time Complexity:** $O(N)$ | **Space:** $O(1)$

### 3. Trapping Rainwater Algorithm
- **Problem solved:** Compute volume of rainwater trapped between elevation bars.
- **Water trapped at bar $i$:** $\min(\text{maxLeft}[i], \text{maxRight}[i]) - \text{height}[i]$
- **Time Complexity:** $O(N)$ | **Space:** $O(N)$ with auxiliary arrays or $O(1)$ with two pointers.

---

## 10. Complexity Reference

| Operation / Problem | Best Case | Average Case | Worst Case | Space Complexity |
|---|---|---|---|---|
| Access / Update | $O(1)$ | $O(1)$ | $O(1)$ | $O(1)$ |
| Linear Search | $O(1)$ | $O(N)$ | $O(N)$ | $O(1)$ |
| Binary Search | $O(1)$ | $O(\log N)$ | $O(\log N)$ | $O(1)$ |
| Subarray Generation | $O(N^3)$ | $O(N^3)$ | $O(N^3)$ | $O(1)$ |
| Prefix Sum Subarray Max | $O(N^2)$ | $O(N^2)$ | $O(N^2)$ | $O(N)$ |
| Kadane's Max Subarray | $O(N)$ | $O(N)$ | $O(N)$ | $O(1)$ |
| Trapping Rainwater | $O(N)$ | $O(N)$ | $O(N)$ | $O(N)$ or $O(1)$ |
| Buy & Sell Stock | $O(N)$ | $O(N)$ | $O(N)$ | $O(1)$ |

---

## 11. Common Mistakes

- **ArrayIndexOutOfBoundsException:** Accessing index `< 0` or `>= arr.length`. Common in loops using `i <= arr.length`.
- **Binary Search Integer Overflow:** Calculating `mid = (start + end) / 2` can overflow integer range when `start + end > 2^{31}-1`. Always write `mid = start + (end - start) / 2`.
- **All Negative Numbers in Kadane's:** Resetting `currSum = 0` blindly when all array elements are negative will return `0` instead of the maximum negative number. Handle all-negative arrays by tracking max single element.
- **Modifying Method Arguments:** Forgetting that array modifications in methods persist in the main thread.

---

## 12. Edge Cases

- **Empty Array (`length == 0`):** Should throw error or return `-1`/`0`.
- **Single Element Array (`length == 1`):** Subarray sum is element itself; stock profit is `0`; trapped water is `0`.
- **Two Elements:** Minimal pair search or stock window.
- **Array with All Identical / Duplicate Values.**
- **Strictly Increasing or Strictly Decreasing Array.**
- **Array with All Negative Values.**

---

## 13. Interview Questions

### Beginner
1. How does array indexing provide $O(1)$ access time in memory?
2. Write a function to reverse an array in-place without using extra space.

### Intermediate
1. Explain how Kadane's Algorithm optimizes maximum subarray sum from $O(N^3)$ to $O(N)$.
2. Why is binary search applicable only on sorted arrays? Trace `mid = start + (end - start) / 2`.

### Advanced
1. Derive the mathematical formula and two-pointer proof for the Trapping Rainwater problem.
2. Given an array of stock prices, explain how to achieve $O(N)$ time and $O(1)$ space for maximum single-transaction profit.

---

## 14. Real-World Applications

- **CPU Cache Buffers:** Contiguous array elements fit in CPU cache lines (L1/L2 cache locality of reference), maximizing memory read performance.
- **Image Processing:** Pixels in digital images are stored as 1D/2D arrays of RGB color bytes.
- **Lookup Tables:** High-frequency routing tables and hash-bucket backbones use arrays for $O(1)$ retrieval.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`ArrayBasics.java`](ArrayBasics.java) | Array creation, input/output operations, length attribute, and element access. |
| [`ArrayPairs.java`](ArrayPairs.java) | Generating and printing all possible pairs in an array ($O(N^2)$ iterations). |
| [`BinarySearch.java`](BinarySearch.java) | Binary Search implementation on sorted arrays ($O(\log N)$ logarithmic search). |
| [`BuyAndSellStocks.java`](BuyAndSellStocks.java) | Single pass greedy stock trading algorithm for maximum profit. |
| [`LargestNumber.java`](LargestNumber.java) | Finding maximum and minimum elements in an array via linear scan. |
| [`LinearSearch.java`](LinearSearch.java) | Sequential linear search finding key index in array ($O(N)$ time). |
| [`MaxSubArrays.java`](MaxSubArrays.java) | Brute-force subarray generation and maximum subarray sum ($O(N^3)$ time). |
| [`MaxSumSubarrayPrefix.java`](MaxSumSubarrayPrefix.java) | Subarray sum optimization using auxiliary Prefix Sum array ($O(N^2)$ time). |
| [`MaxSumSubarrayKadanes.java`](MaxSumSubarrayKadanes.java) | Kadane's Algorithm for maximum subarray sum in $O(N)$ time and $O(1)$ space. |
| [`PrintSubarrays.java`](PrintSubarrays.java) | Printing all contiguous subarrays and calculating total subarray count. |
| [`ReverseArray.java`](ReverseArray.java) | In-place array reversal using Two-Pointers technique ($O(N)$ time, $O(1)$ space). |
| [`TrappingRainwater.java`](TrappingRainwater.java) | Trapping Rainwater algorithm using auxiliary `leftMax` and `rightMax` bounds ($O(N)$ time). |

---

## 16. Related Topics

### Prerequisites
- Java Basics (loops, variables, conditions).

### Related Topics
- 2D Arrays (Matrices & Grids).
- Sorting Algorithms (Bubble, Selection, Insertion, Counting).
- ArrayList (Dynamic resizable arrays).

### Next Topics
- Two Pointers & Sliding Window techniques.
- Linked Lists (non-contiguous dynamic memory).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for array indexing, Kadane's visualization, or Trapping Rainwater water columns here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
