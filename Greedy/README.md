# Greedy Algorithms

## 1. What Is This?

A Greedy Algorithm is an algorithmic paradigm that builds up a solution piece by piece, always making the choice that offers the **most immediate / local benefit (locally optimal choice)** without worrying about future consequences or backtracking.

Unlike Dynamic Programming or Backtracking, a Greedy algorithm NEVER reconsiders past choices. Once a decision is made, it is permanent.

While Greedy algorithms do not work for all optimization problems, when applicable, they are extremely fast and efficient.

---

## 2. Core Idea

A problem can be solved using a Greedy approach if it satisfies two key properties:

1. **Greedy Choice Property:** A global optimum can be reached by making locally optimal (greedy) choices at each step.
2. **Optimal Substructure:** An optimal solution to the problem contains optimal solutions to its sub-problems.

```text
       ┌──────────────────────────────────────────────────┐
       │             Current State / Choices              │
       └────────────────────────┬─────────────────────────┘
                                │
                                ▼  Make Locally Optimal Choice!
       ┌──────────────────────────────────────────────────┐
       │   Commit to Best Immediate Option (NO UNDO!)     │
       └────────────────────────┬─────────────────────────┘
                                │
                                ▼
       ┌──────────────────────────────────────────────────┐
       │           Proceed to Next Sub-problem            │
       └──────────────────────────────────────────────────┘
```

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Greedy Choice** | Selecting the option that looks best at the current moment. |
| **Activity Selection** | Problem of scheduling the maximum number of non-overlapping tasks sharing a single resource. |
| **End Time Sorting** | Key greedy heuristic: sorting activities by their completion/end times in ascending order. |
| **Feasible Choice** | A choice that satisfies problem constraints without conflicting with previously selected choices. |

---

## 4. Visual / Mental Model

### Activity Selection Sorting Strategy

Suppose we have 3 activities:

```text
  Activity 1: [======] (End = 4)
  Activity 2: [==============] (End = 8)
  Activity 3:     [======] (End = 5)
```

Why sort by **END TIME** instead of start time or duration?
Sorting by earliest end time finishes the selected activity as early as possible, **leaving the maximum possible remaining time window** free for subsequent activities!

---

## 5. Operations / Techniques

### Activity Selection Algorithm Steps

1. Sort all activities by their **End Time** in ascending order.
2. Select the first activity (earliest end time) and record its end time `lastEnd`.
3. Loop through remaining activities $i$:
   - If `start[i] >= lastEnd`:
     - Select activity $i$.
     - Update `lastEnd = end[i]`.

---

## 6. Worked Examples

### Worked Example: Activity Selection Step Trace

**Input Activities:**

| Activity | A1 | A2 | A3 | A4 | A5 | A6 |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **Start** | 1 | 3 | 0 | 5 | 8 | 5 |
| **End** | 2 | 4 | 6 | 7 | 9 | 9 |

- **Step 1:** Sort by end time:
  - `A1: (1, 2)`
  - `A2: (3, 4)`
  - `A3: (0, 6)`
  - `A4: (5, 7)`
  - `A5: (8, 9)`
  - `A6: (5, 9)`
- **Step 2:** Select `A1 (1, 2)`. Count $= 1$. `lastEnd = 2`.
- **Step 3:** Inspect `A2 (3, 4)`: `start (3) >= lastEnd (2)` $\implies$ **Select `A2`!** Count $= 2$. `lastEnd = 4`.
- **Step 4:** Inspect `A3 (0, 6)`: `start (0) < lastEnd (4)` $\implies$ Reject!
- **Step 5:** Inspect `A4 (5, 7)`: `start (5) >= lastEnd (4)` $\implies$ **Select `A4`!** Count $= 3$. `lastEnd = 7`.
- **Step 6:** Inspect `A5 (8, 9)`: `start (8) >= lastEnd (7)` $\implies$ **Select `A5`!** Count $= 4$. `lastEnd = 9`.
- **Step 7:** Inspect `A6 (5, 9)`: `start (5) < lastEnd (9)` $\implies$ Reject!
- **Result:** **Maximum 4 non-overlapping activities selected: `[A1, A2, A4, A5]`**.

---

## 7. Java Implementation Concepts

- **Sorting 2D Arrays / Objects by Custom Criterion:**
  ```java
  int[][] activities = {{1, 2}, {3, 4}, {0, 6}, {5, 7}, {8, 9}, {5, 9}};

  // Sort matrix by column 1 (End Time) using Lambda Comparator
  Arrays.sort(activities, Comparator.comparingInt(o -> o[1]));
  ```
- **Pre-Sorted Input Optimization:** If activities are ALREADY sorted by end time, the algorithm executes in pure $O(N)$ linear time without requiring array sorting!

---

## 8. Problem-Solving Patterns

### Pattern 1: End-Time / Deadline Sorting
- **When to think of it:** Activity selection, interval scheduling, task deadline optimization.
- **Mental Approach:** Sort intervals by end time. Pick next interval whose start time $\ge$ previous selected end time.

---

## 9. Algorithms

### Activity Selection Algorithm
- **Problem solved:** Maximize total number of non-overlapping activities.
- **Pseudocode:**
  ```text
  function maxActivities(start, end):
      n = start.length
      activities = 2D array storing (start, end, originalIndex)
      sort activities by end time ascending
      
      count = 1
      selectedList.add(activities[0].index)
      lastEnd = activities[0].end

      for i = 1 to n - 1:
          if activities[i].start >= lastEnd:
              count++
              selectedList.add(activities[i].index)
              lastEnd = activities[i].end
      return count
  ```
- **Time Complexity:** $O(N \log N)$ (Unsorted) / $O(N)$ (Pre-sorted) | **Space:** $O(N)$

---

## 10. Complexity Reference

| Operation / Case | Time Complexity | Space Complexity |
|---|---|---|
| Sorting Activities by End Time | $O(N \log N)$ | $O(N)$ |
| Linear Activity Scan | $O(N)$ | $O(1)$ |
| Total Activity Selection (Unsorted) | $O(N \log N)$ | $O(N)$ |
| Total Activity Selection (Pre-Sorted)| $O(N)$ | $O(1)$ extra |

---

## 11. Common Mistakes

- **Sorting by Start Time Instead of End Time:** Sorting by start time fails when a long activity starts very early (e.g. `(0, 100)`), blocking all subsequent short activities!
- **Sorting by Duration Instead of End Time:** Sorting by shortest duration fails on overlapping boundary cases.
- **Strict Inequality Check:** Writing `start[i] > lastEnd` instead of `start[i] >= lastEnd` (excluding valid back-to-back activities where one ends at time $t$ and next starts at time $t$).

---

## 12. Edge Cases

- **Zero Overlapping Activities:** All activities can be selected ($N$).
- **Fully Overlapping Activities (All same start & end):** Only 1 activity can be selected.
- **Single Activity ($N = 1$).**

---

## 13. Interview Questions

### Beginner
1. What is the fundamental difference between a Greedy algorithm and Dynamic Programming?
2. Why must activities be sorted by End Time rather than Start Time in the Activity Selection problem?

### Intermediate
1. Prove why the Greedy choice of picking the activity with the earliest end time is mathematically optimal.
2. How does pre-sorting input activities reduce time complexity from $O(N \log N)$ to $O(N)$?

### Advanced
1. Compare Fractional Knapsack (Greedy optimal $O(N \log N)$) vs 0/1 Knapsack (Requires Dynamic Programming $O(N \times W)$).

---

## 14. Real-World Applications

- **CPU Job Scheduling:** Processing real-time task queues with tight deadlines.
- **Conference Room Booking Systems:** Maximizing total meeting bookings in shared office spaces.
- **Network Bandwidth Allocation:** Scheduling transmission bursts in communication channels.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`ActivitySelection.java`](ActivitySelection.java) | Complete Activity Selection algorithm sorting by end time (both pre-sorted $O(N)$ and unsorted $O(N \log N)$ cases using custom Comparators). |

---

## 16. Related Topics

### Prerequisites
- 1D Arrays, Sorting Algorithms, & Java Comparators.

### Related Topics
- Heaps & Priority Queues (Min-Heap for Greedy choices).
- Dynamic Programming (0/1 Knapsack & Longest Increasing Subsequence).

### Next Topics
- Binary Trees & Priority Queues (Heaps).
- Fractional Knapsack & Minimum Spanning Trees (Kruskal / Prim).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add visual notes or timeline diagram for Activity Selection sorting here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
