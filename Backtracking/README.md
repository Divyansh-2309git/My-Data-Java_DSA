# Backtracking Algorithms

## 1. What Is This?

Backtracking is an algorithmic technique for solving problems incrementally by trying to build a solution one piece at a time. If at any step a choice leads to a dead-end or violates problem constraints, the algorithm **undoes the choice (backtracks)** and returns to the previous decision point to try an alternative path.

Backtracking is systematically searching through a state space tree of all possible choices (a brute-force search with pruning).

---

## 2. Core Idea

Backtracking relies on three main concepts:

1. **Choice:** What decision can be made at the current step? (e.g., include or exclude a character, place a queen or move to next column).
2. **Constraints:** Rules that restrict valid choices. If a choice violates a constraint, prune the branch immediately!
3. **Goal:** The target state where a valid solution is completed (base case).

```text
                            ┌────────────────────────┐
                            │    Root Decision Node  │
                            └───────────┬────────────┘
                                        │
                 ┌──────────────────────┴──────────────────────┐
                 │ Choice 1                                    │ Choice 2
                 ▼                                             ▼
       ┌──────────────────┐                          ┌──────────────────┐
       │   State Branch   │                          │   State Branch   │
       └─────────┬────────┘                          └─────────┬────────┘
                 │ (Constraint Violated!)                      │ (Valid Goal!)
                 ▼                                             ▼
          [ DEAD END! ]                                 [ SOLUTION FOUND ]
          (UNDO CHOICE / BACKTRACK!)
```

The defining characteristic of backtracking code is the **UNDO STEP** executed immediately after the recursive call returns!

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **State Space Tree** | A tree representing all possible configurations/states reachable from the initial state. |
| **Pruning** | Cutting off branches of the state space tree that cannot lead to a valid solution. |
| **Backtrack Step** | Reverting state changes (e.g., un-setting a variable, restoring array values) after recursive return. |
| **Subset** | A selection of elements from a set. A set of size $N$ has $2^N$ total subsets. |

---

## 4. Visual / Mental Model

### Subset Generation Tree for String `"abc"`

At each character index $i$, we make a binary choice: **Include** (Yes) or **Exclude** (No).

```text
                                    "" (index 0)
                              /                    \
                     Include 'a'                 Exclude 'a'
                        /                            \
                    "a" (idx 1)                    "" (idx 1)
                   /           \                  /          \
             Inc 'b'         Exc 'b'        Inc 'b'        Exc 'b'
               /               \              /               \
          "ab" (idx 2)     "a" (idx 2)   "b" (idx 2)      "" (idx 2)
           /       \        /       \     /       \        /       \
         "abc"    "ab"    "ac"     "a"  "bc"     "b"     "c"       ""
```

Total leaf nodes $= 2^3 = 8$ subsets!

---

## 5. Operations / Techniques

### 1. Array Backtracking (Mutating Array on Return Path)
On forward recursive call: `arr[i] = i + 1`.
After recursive return (backtrack step): `arr[i] = arr[i] - 2`.
Shows how modifications on the unwinding stack change the array state back!

### 2. Subset Generation (Inclusion / Exclusion Pattern)
For a string of length $N$:
- Branch 1 (Yes): `findSubsets(str, ans + str.charAt(i), i + 1)`
- Branch 2 (No): `findSubsets(str, ans, i + 1)`

---

## 6. Worked Examples

### Worked Example 1: Backtracking on Array

**Input Array:** `arr` of size 5 initialized to zeros.

- **Forward Path (Going down call stack):**
  - $i=0$: `arr[0] = 1` $\to$ call $i=1$
  - $i=1$: `arr[1] = 2` $\to$ call $i=2$
  - $i=2$: `arr[2] = 3` $\to$ call $i=3$
  - $i=3$: `arr[3] = 4` $\to$ call $i=4$
  - $i=4$: `arr[4] = 5` $\to$ Base case reached! Print `[1, 2, 3, 4, 5]`.
- **Return Path (Unwinding stack & Backtracking):**
  - $i=4$: `arr[4] = 5 - 2 = 3`
  - $i=3$: `arr[3] = 4 - 2 = 2`
  - $i=2$: `arr[2] = 3 - 2 = 1`
  - $i=1$: `arr[1] = 2 - 2 = 0`
  - $i=0$: `arr[0] = 1 - 2 = -1`
- **Result Array after Backtracking completes:** **`[-1, 0, 1, 2, 3]`**

---

## 7. Java Implementation Concepts

- **Pass-by-Value with Reference Objects:** When passing dynamic arrays or `StringBuilder` objects into backtracking methods, state changes persist across recursive calls! You MUST explicitly undo changes (e.g. `sb.deleteCharAt(sb.length() - 1)` or `list.remove(list.size() - 1)`).
- **String Immutability Shortcut:** Passing immutable `String` concats (`ans + str.charAt(i)`) creates new String instances automatically, effectively performing automatic backtracking for string objects without explicit undo calls!

---

## 8. Problem-Solving Patterns

### Pattern 1: Pick / Don't Pick (Inclusion / Exclusion)
- **When to think of it:** Generating all subsets, power set, combinations, 0/1 Knapsack decision tree.
- **Mental Approach:** Two recursive calls at each index: one including element, one excluding element.

### Pattern 2: Explicit Undo Step
- **When to think of it:** Modifying a shared global state (Array, Board grid, Visited set).
- **Mental Approach:**
  ```java
  state.makeChoice();
  backtrack(index + 1);
  state.undoChoice(); // ALWAYS UNDO STATE MUTATION!
  ```

---

## 9. Algorithms

### Subset Generation Algorithm
- **Problem solved:** Print all $2^N$ subsets of a given string.
- **Pseudocode:**
  ```text
  function findSubsets(str, ans, i):
      if i == str.length():
          print(ans == "" ? "null" : ans)
          return
      // Choice 1: Yes (Include character)
      findSubsets(str, ans + str.charAt(i), i + 1)
      // Choice 2: No (Exclude character)
      findSubsets(str, ans, i + 1)
  ```
- **Time Complexity:** $O(2^N \times N)$ | **Space Complexity:** $O(N)$ call stack depth

---

## 10. Complexity Reference

| Problem / Technique | Time Complexity | Space Complexity |
|---|---|---|
| Array Backtracking Mutation | $O(N)$ | $O(N)$ call stack |
| Find All Subsets of String | $O(2^N)$ | $O(N)$ call stack depth |
| Find All Permutations | $O(N \times N!)$ | $O(N)$ call stack depth |
| N-Queens Problem | $O(N!)$ | $O(N)$ board & stack |
| Sudoku Solver | $O(9^{N^2})$ worst | $O(N^2)$ grid stack |

---

## 11. Common Mistakes

- **Forgetting the Undo Step:** Mutating a shared collection (like an `ArrayList`) without removing the item after the recursive call returns. This leaves stale choices in future branches!
- **Modifying Loop Counters:** Accidentally mutating loop indices inside the backtracking loop.
- **Incorrect Base Case Return:** Forgetting to `return` after hitting the base case, allowing execution to fall through into invalid recursive calls.

---

## 12. Edge Cases

- **Empty Input (`""` or `length == 0`):** Should produce empty set `""` / `null`.
- **String with Duplicate Characters:** Can produce duplicate subsets if not filtered.
- **Max Recursion Limits:** Inputs with $N > 25$ generate $> 3.3 \times 10^7$ combinations, leading to execution timeouts.

---

## 13. Interview Questions

### Beginner
1. What is the fundamental difference between standard Recursion and Backtracking?
2. Why is an explicit "undo" step necessary when modifying shared arrays in backtracking?

### Intermediate
1. Explain how the inclusion/exclusion decision tree generates $2^N$ subsets for a set of size $N$.
2. Trace how array values change on the forward path versus the unwinding return path in `BacktrackOnArrays.java`.

### Advanced
1. How does Pruning improve performance in backtracking algorithms like N-Queens or Sudoku Solver?
2. Compare time and space complexities of Subset Generation ($O(2^N)$) versus Permutation Generation ($O(N!)$).

---

## 14. Real-World Applications

- **Sudoku & Crossword Solvers:** Automated puzzle solvers using constraint propagation.
- **Combinatorial Optimization:** Automated route planning, resource allocation, and job scheduling.
- **Circuit Design & Chip Layout:** Finding valid non-overlapping component placements on silicon wafers.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`BacktrackOnArrays.java`](BacktrackOnArrays.java) | Array assignment during call stack depth expansion and value reduction during stack unwinding. |
| [`FindSubsets.java`](FindSubsets.java) | Inclusion/Exclusion decision tree algorithm generating all $2^N$ subsets of a string. |

---

## 16. Related Topics

### Prerequisites
- Recursion Basics & Call Stack Mechanics.

### Related Topics
- Divide & Conquer.
- Depth-First Search (DFS) on Graphs & Trees.

### Next Topics
- N-Queens & Grid Backtracking.
- Dynamic Programming & Branch-and-Bound.

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for State Space Tree pruning or Subset Inclusion/Exclusion tree here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
