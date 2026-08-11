# Dynamic Arrays & ArrayList Patterns

## 1. What Is This?

`ArrayList` is part of the Java Collections Framework (`java.util.ArrayList`). It represents a dynamically resizable array. While standard Java arrays have a fixed size defined at creation, an `ArrayList` automatically grows and shrinks as elements are added or removed.

This module covers core `ArrayList` operations, multidimensional list structures, and classical two-pointer algorithmic patterns like **Container With Most Water**, **Pair Sum in Sorted Array**, and **Pair Sum in Rotated Array**.

---

## 2. Core Idea

Internally, an `ArrayList` wraps a standard primitive array. When the internal array becomes full:

1. A new, larger array (typically $1.5\times$ or $2\times$ original capacity) is allocated in memory.
2. Existing elements are copied into the new array.
3. The old array reference is garbage collected.

```text
Initial Capacity: 4      [ 10 ][ 20 ][ 30 ][ 40 ]   (FULL!)
                          │
Adding 5th element ──►    ▼ Resize (Capacity -> 8)
New Array:               [ 10 ][ 20 ][ 30 ][ 40 ][ 50 ][    ][    ][    ]
```

Although resizing requires an $O(N)$ copy operation, resizing happens infrequently (logarithmically), making `add()` run in **Amortized $O(1)$** constant time!

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Capacity** | The size of the internal array buffer currently allocated in memory. |
| **Size** | The actual number of elements currently stored in the `ArrayList` (`list.size()`). |
| **Amortized Time** | Average time per operation over a long sequence of operations. |
| **Wrapper Class** | Object representations of primitive types required by Java Generics (`Integer`, `Double`, `Boolean`). |
| **Autoboxing** | Automatic conversion of primitive types to their corresponding wrapper object (e.g. `int` to `Integer`). |
| **Pivot / Break Point** | The index in a rotated sorted array where element value drops (`arr[i] > arr[i+1]`). |

---

## 4. Visual / Mental Model

### Container With Most Water (Two Pointers)

Water volume between two vertical lines at indices `i` and `j` is bounded by the shorter line:

$$\text{Area} = \min(\text{height}[i], \text{height}[j]) \times (j - i)$$

```text
  Height
    8 |     |                           |
    7 |     |                           |       |
    6 |     |   |                       |       |
    5 |     |   |   |                   |   |   |
    4 |     |   |   |   |               |   |   |
    3 |     |   |   |   |   |           |   |   |
    2 |     |   |   |   |   |   |       |   |   |
    1 |     |   |   |   |   |   |   |   |   |   |
    0 └─────┴───┴───┴───┴───┴───┴───┴───┴───┴───┴─── Index
            0   1   2   3   4   5   6   7   8
          left                         right
```

Moving the taller line cannot increase the area because width decreases while height remains bottlenecked by the shorter line! Therefore, we **always move the pointer pointing to the shorter line**.

---

## 5. Operations / Techniques

| Operation | Java API | Time Complexity | Space Complexity |
|---|---|---|---|
| **Add at End** | `list.add(value)` | Amortized $O(1)$ | $O(1)$ |
| **Get Element** | `list.get(index)` | $O(1)$ | $O(1)$ |
| **Set / Replace** | `list.set(index, value)` | $O(1)$ | $O(1)$ |
| **Remove by Index** | `list.remove(index)` | $O(N)$ (requires shifting) | $O(1)$ |
| **Contains Value** | `list.contains(value)` | $O(N)$ (linear search) | $O(1)$ |
| **Sort List** | `Collections.sort(list)` | $O(N \log N)$ | $O(N)$ |
| **Multi-dimensional**| `ArrayList<ArrayList<T>>` | Grid access $O(1)$ | $O(N \times M)$ |

---

## 6. Worked Examples

### Worked Example 1: Container With Most Water

**Heights:** `[1, 8, 6, 2, 5, 4, 8, 3, 7]`

- **Step 1:** Initialize `lp = 0` (`h=1`), `rp = 8` (`h=7`), `maxWater = 0`.
  - $\text{width} = 8 - 0 = 8$. $\text{minH} = \min(1, 7) = 1$. $\text{water} = 8 \times 1 = 8$.
  - $\text{maxWater} = \max(0, 8) = 8$.
  - Shorter is at `lp` ($1 < 7$) $\implies$ `lp++` (now `lp = 1`).
- **Step 2:** `lp = 1` (`h=8`), `rp = 8` (`h=7`).
  - $\text{width} = 8 - 1 = 7$. $\text{minH} = \min(8, 7) = 7$. $\text{water} = 7 \times 7 = 49$.
  - $\text{maxWater} = \max(8, 49) = 49$.
  - Shorter is at `rp` ($7 < 8$) $\implies$ `rp--` (now `rp = 7`).
- **Step 3:** Continue inward pointer movement...
- **Result:** `maxWater = 49` (formed between index 1 and index 8).

### Worked Example 2: Pair Sum in Rotated Sorted Array

**Input List:** `[11, 15, 6, 8, 9, 10]`, Target = `16`

- **Step 1:** Find pivot index where sequence breaks: `15 > 6` at index `1`.
  - Smallest element is at `lp = 2` (`val = 6`).
  - Largest element is at `rp = 1` (`val = 15`).
- **Step 2:**
  - $\text{sum} = 6 + 15 = 21$. Target = `16`.
  - $\text{sum} > \text{target} \implies$ Move `rp` backward circularly: `rp = (rp - 1 + n) % n = (1 - 1 + 6) % 6 = 0` (`val = 11`).
- **Step 3:**
  - $\text{sum} = 6 + 11 = 17$. Target = `16`.
  - $\text{sum} > \text{target} \implies$ Move `rp` backward circularly: `rp = (0 - 1 + 6) % 6 = 5` (`val = 10`).
- **Step 4:**
  - $\text{sum} = 6 + 10 = 16$. Target = `16`.
  - $\text{sum} == \text{target} \implies$ **Found pair `(6, 10)` at indices `(2, 5)`!**

---

## 7. Java Implementation Concepts

- **Generics Syntax:** `ArrayList<Integer> list = new ArrayList<>();` (Diamond operator `<>` avoids redundant type declaration).
- **Primitive Restriction:** Cannot store primitives directly (`ArrayList<int>` is invalid). Must use wrapper types (`ArrayList<Integer>`).
- **Swapping Elements in ArrayList:**
  ```java
  public static void swap(ArrayList<Integer> list, int idx1, int idx2) {
      int temp = list.get(idx1);
      list.set(idx1, list.get(idx2));
      list.set(idx2, temp);
  }
  ```
- **2D ArrayList Declaration:**
  ```java
  ArrayList<ArrayList<Integer>> mainList = new ArrayList<>();
  ArrayList<Integer> row1 = new ArrayList<>();
  mainList.add(row1);
  ```

---

## 8. Problem-Solving Patterns

### Pattern 1: Two Pointers (Inward Convergence)
- **When to think of it:** Sorted pair sum, Container with most water.
- **Mental Approach:** Set `left = 0`, `right = list.size() - 1`. Shrink search space by moving left rightward or right leftward based on condition.

### Pattern 2: Circular Modular Pointer Movement
- **When to think of it:** Sorted array rotated at an unknown pivot.
- **Mental Approach:** Next right element: `(i + 1) % n`. Previous left element: `(i - 1 + n) % n`.

---

## 9. Algorithms

### Circular Two-Pointer Pair Sum
- **Problem solved:** Find if any pair in sorted & rotated list sums to target.
- **Pseudocode:**
  ```text
  function pairSumRotated(list, target):
      n = list.size()
      pivot = findPivot(list) // index where list[i] > list[i+1]
      lp = pivot + 1 // smallest element
      rp = pivot     // largest element
      while lp != rp:
          sum = list.get(lp) + list.get(rp)
          if sum == target return true
          else if sum < target lp = (lp + 1) % n
          else rp = (rp - 1 + n) % n
      return false
  ```
- **Time Complexity:** $O(N)$ | **Space Complexity:** $O(1)$

---

## 10. Complexity Reference

| Operation | Time Complexity | Space Complexity |
|---|---|---|
| Index Access (`get(i)`) | $O(1)$ | $O(1)$ |
| Value Replace (`set(i, v)`) | $O(1)$ | $O(1)$ |
| Add at End (`add(v)`) | Amortized $O(1)$ | $O(1)$ |
| Insert at Index (`add(i, v)`)| $O(N)$ | $O(1)$ |
| Remove at Index (`remove(i)`)| $O(N)$ | $O(1)$ |
| Container With Most Water | $O(N)$ | $O(1)$ |
| Pair Sum 1 (Sorted) | $O(N)$ | $O(1)$ |
| Pair Sum 2 (Rotated) | $O(N)$ | $O(1)$ |

---

## 11. Common Mistakes

- **ConcurrentModificationException:** Removing elements from an `ArrayList` inside an enhanced `for` loop (`for (int val : list)`). Use an `Iterator` or loop backward by index instead.
- **Off-by-One in `remove(int index)` vs `remove(Object obj)`:** Calling `list.remove(2)` removes element at **index 2**, not value `2`. To remove Integer value `2`, call `list.remove(Integer.valueOf(2))`.
- **Negative Modulo Result in Java:** In Java, `-1 % n` evaluates to `-1` (not positive!). Always add `n` before taking modulo: `(i - 1 + n) % n`.

---

## 12. Edge Cases

- **Empty List / Single Element:** Size $< 2$ cannot form pairs or containers.
- **List with No Pivot Break:** Rotated sorted list with zero rotation (already normally sorted).
- **Duplicate Elements in List.**
- **Large List Sizing:** Pre-specify capacity `new ArrayList<>(1000)` if target size is known to avoid dynamic reallocations.

---

## 13. Interview Questions

### Beginner
1. What is the difference between an Array and an `ArrayList` in Java?
2. Explain the concept of Amortized $O(1)$ time complexity for `ArrayList.add()`.

### Intermediate
1. How does Java Autoboxing and Unboxing work when storing primitives in an `ArrayList`?
2. Explain why `ArrayList.remove(0)` takes $O(N)$ time while `ArrayList.remove(list.size() - 1)` takes $O(1)$ time.

### Advanced
1. Derive the mathematical proof for why the two-pointer approach for Container With Most Water is optimal and never misses the global maximum.
2. How do you implement two-pointer pair sum on a rotated sorted array using modular arithmetic?

---

## 14. Real-World Applications

- **Dynamic Data Buffers:** Storing dynamic API HTTP query results when record count is unknown in advance.
- **UI Element Lists:** Rendering dynamic feeds (e.g., social media posts, chat messages) where items are continuously appended.
- **Graph Adjacency Lists:** Representing dynamic graph edges using `ArrayList<ArrayList<Integer>>`.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`ArrayListBasics.java`](ArrayListBasics.java) | Creation, basic operations (`add`, `get`, `set`, `remove`), size retrieval, and list reversal. |
| [`ContainerWithMostWater.java`](ContainerWithMostWater.java) | Two-pointer optimal area calculation solving Container With Most Water in $O(N)$ time. |
| [`FindMaximum.java`](FindMaximum.java) | Linear scan finding global maximum element in an `ArrayList`. |
| [`MultiDimensionalArrayList.java`](MultiDimensionalArrayList.java) | 2D dynamic nested list (`ArrayList<ArrayList<Integer>>`) grid creation and traversal. |
| [`PairSumSorted.java`](PairSumSorted.java) | Inward two-pointer linear search for target pair sum in a sorted `ArrayList` ($O(N)$ time). |
| [`PairSumRotated.java`](PairSumRotated.java) | Circular modular two-pointer search finding target pair sum in a rotated sorted `ArrayList` ($O(N)$ time). |
| [`SortArrayList.java`](SortArrayList.java) | Sorting `ArrayList` in ascending and descending order using `Collections.sort()`. |
| [`SwapElements.java`](SwapElements.java) | Swapping two elements at specified indices in an `ArrayList`. |

---

## 16. Related Topics

### Prerequisites
- 1D Arrays & Two Pointers concept.

### Related Topics
- Java Collections Framework.
- Vector & LinkedList performance comparisons.

### Next Topics
- Strings & StringBuilder (character sequence data).
- Linked Lists (pointer-based dynamic structures).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add visual notes for ArrayList internal buffer expansion or Two-Pointer Container step traces.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
