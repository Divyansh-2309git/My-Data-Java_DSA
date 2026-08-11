# Practice Problems & Skill Verification

## 1. What Is This?

The `Practice/` directory serves as a dedicated hands-on workspace within this repository for reinforcing fundamental DSA topics, testing problem-solving patterns, and validating environment setup.

While primary topic modules explain core concepts step-by-step, this module contains revision exercises across **Arrays**, **Recursion**, and **Environment Testing**.

---

## 2. Core Idea

Reinforcing algorithms requires moving from passive reading to active implementation. The exercises in this folder focus on core patterns:

1. **Array Trend Verification:** Single pass linear scans to test monotonicity and duplicate detection.
2. **Recursive Base Case & Unwinding:** Practicing call stack control, index collections, and substring combinatorial logic.
3. **Environment Verification:** Ensuring local JDK configuration operates cleanly.

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Monotonic Array** | An array whose elements are entirely non-increasing ($a[i] \ge a[i+1]$) or non-decreasing ($a[i] \le a[i+1]$). |
| **Contains Duplicate** | Problem checking whether any value appears at least twice in an array. |
| **Contiguous Substring** | A continuous slice of a string. Matching start and end characters requires $S[i] == S[j]$. |
| **Environment Check** | A simple program (`EnvironmentTest.java`) ensuring `javac` and `java` CLI run without errors. |

---

## 4. Visual / Mental Model

### Monotonic Array Inspection (Single Pass $O(N)$)

We maintain two boolean flags: `inc = true` and `dec = true`.

```text
Array: [ 1 , 2 , 2 , 3 ]
  Index 0->1 (1->2): 1 <= 2 (Valid Increasing) | 1 >= 2 (FALSE! dec = false)
  Index 1->2 (2->2): 2 <= 2 (Valid Increasing) | dec is already false
  Index 2->3 (2->3): 2 <= 3 (Valid Increasing) | dec is already false

Result: inc = true, dec = false => inc || dec is TRUE! Array is Monotonic!
```

---

## 5. Operations / Techniques

### 1. Monotonic Array Check ($O(N)$ time, $O(1)$ space)
Iterate $i$ from `0` to `n-2`.
- If `arr[i] > arr[i+1]`, set `inc = false`.
- If `arr[i] < arr[i+1]`, set `dec = false`.
- Return `inc || dec`.

### 2. Recursive Index Collection
Find all occurrence indices of a key in an array recursively by returning an array or list during stack unwinding.

---

## 6. Worked Examples

### Worked Example: Recursive Contiguous Substrings Count

**Input String:** `"aba"`

- **Goal:** Count substrings where starting character equals ending character.
- **Valid Substrings:**
  1. `"a"` (idx 0 to 0) -> Starts 'a', ends 'a' (VALID)
  2. `"b"` (idx 1 to 1) -> Starts 'b', ends 'b' (VALID)
  3. `"a"` (idx 2 to 2) -> Starts 'a', ends 'a' (VALID)
  4. `"aba"` (idx 0 to 2) -> Starts 'a', ends 'a' (VALID)
  5. `"ab"` (idx 0 to 1) -> Starts 'a', ends 'b' (INVALID)
  6. `"ba"` (idx 1 to 2) -> Starts 'b', ends 'a' (INVALID)
- **Result:** **Total Count = `4`**.

---

## 7. Java Implementation Concepts

- **Multi-Directory Package Navigation:** Java files inside `Practice/Arrays/`, `Practice/Recursion/`, and `Practice/Test/` reflect modular folder partitioning.
- **Compilation Commands:**
  ```bash
  javac Practice/Arrays/MonotonicArrayList.java
  java Practice.Arrays.MonotonicArrayList
  ```

---

## 8. Problem-Solving Patterns

### Pattern 1: Single Pass Boolean Flag Accumulator
- **When to think of it:** Monotonic checks, array sorted checks.
- **Mental Approach:** Track trends with boolean flags in a single $O(N)$ loop without early break errors.

---

## 9. Algorithms

### Monotonic Array Check Algorithm
- **Pseudocode:**
  ```text
  function isMonotonic(arr):
      inc = true, dec = true
      for i = 0 to arr.length - 2:
          if arr[i] > arr[i+1] inc = false
          if arr[i] < arr[i+1] dec = false
      return inc or dec
  ```
- **Time Complexity:** $O(N)$ | **Space Complexity:** $O(1)$

---

## 10. Complexity Reference

| Practice Problem | Time Complexity | Space Complexity |
|---|---|---|
| `MonotonicArrayList.java` | $O(N)$ | $O(1)$ |
| `ContainsDuplicatePractice.java` | $O(N)$ (Hash) / $O(N \log N)$ (Sort) | $O(N)$ or $O(1)$ |
| `BubbleSortPractice.java` | $O(N^2)$ | $O(1)$ |
| `PrintIncreasingPractice.java` | $O(N)$ | $O(N)$ call stack |
| `PrintDecreasingPractice.java` | $O(N)$ | $O(N)$ call stack |
| `FindAllOccurrencesPractice.java` | $O(N)$ | $O(N)$ call stack |
| `ContiguousSubstrings.java` | $O(2^N)$ or $O(N^2)$ | $O(N)$ call stack |
| `EnvironmentTest.java` | $O(1)$ | $O(1)$ |

---

## 11. Common Mistakes

- **Incorrect Loop Bounds in Monotonic Check:** Checking `arr[i+1]` when $i = n-1$, throwing `ArrayIndexOutOfBoundsException`.
- **Duplicate Counting in Substrings:** Counting overlapping substring boundaries twice.

---

## 12. Edge Cases

- **Empty / Single Element Array:** Always monotonic ($O(1)$ true).
- **Array with All Equal Values (`[2, 2, 2, 2]`):** Both `inc` and `dec` remain `true`.

---

## 13. Interview Questions

### Beginner
1. How do you check if an array is monotonic in a single $O(N)$ pass?
2. Explain how `EnvironmentTest.java` verifies JDK execution.

### Intermediate
1. Differentiate between using Hashing ($O(N)$ time, $O(N)$ space) vs Sorting ($O(N \log N)$ time, $O(1)$ space) for the Contains Duplicate problem.

---

## 14. Real-World Applications

- **Stock Market Trend Tracking:** Detecting strictly upward (bullish) or downward (bearish) monotonic price movements.
- **Automated Sanity Testing:** Unit test files verifying compilation environment integrity.

---

## 15. Repository Implementations

| Subdirectory | File | Concept Demonstrated |
|---|---|---|
| `Practice/Arrays/` | [`MonotonicArrayList.java`](Arrays/MonotonicArrayList.java) | Single-pass $O(N)$ verification of non-increasing or non-decreasing trends in an ArrayList. |
| `Practice/Arrays/` | [`ContainsDuplicatePractice.java`](Arrays/ContainsDuplicatePractice.java) | Duplicate detection logic in array elements. |
| `Practice/Arrays/` | [`BubbleSortPractice.java`](Arrays/BubbleSortPractice.java) | Practice implementation of Bubble Sort bubbling mechanism. |
| `Practice/Recursion/` | [`PrintIncreasingPractice.java`](Recursion/PrintIncreasingPractice.java) | Head recursive practice printing numbers 1 to N. |
| `Practice/Recursion/` | [`PrintDecreasingPractice.java`](Recursion/PrintDecreasingPractice.java) | Tail recursive practice printing numbers N to 1. |
| `Practice/Recursion/` | [`FindAllOccurrencesPractice.java`](Recursion/FindAllOccurrencesPractice.java) | Recursive scan collecting all index positions of a target key. |
| `Practice/Recursion/` | [`ContiguousSubstrings.java`](Recursion/ContiguousSubstrings.java) | Recursive count of substrings with identical start and end characters. |
| `Practice/Test/` | [`EnvironmentTest.java`](Test/EnvironmentTest.java) | Environment verification test printing system confirmation. |

---

## 16. Related Topics

### Prerequisites
- Arrays, ArrayList, & Recursion Basics.

### Related Topics
- Unit Testing & Test-Driven Development (TDD).

### Next Topics
- LeetCode Problem Solving (Separate Repository).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add visual notes or practice flowcharts here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
