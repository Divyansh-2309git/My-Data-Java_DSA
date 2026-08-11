# 2D Arrays & Matrix Algorithms

## 1. What Is This?

A 2D Array (Matrix) is a grid-like collection of elements arranged in rows and columns. In Java, a 2D array is implemented as an "array of arrays", where each row is itself a 1D array.

Matrices are heavily used in image processing, game development (boards, grids), graph representation (adjacency matrices), linear algebra, and grid-based dynamic programming problems.

---

## 2. Core Idea

Accessing an element in a 2D matrix requires two indices: `matrix[row][col]`.

- `matrix.length`: Gives total number of **rows** ($N$).
- `matrix[0].length`: Gives total number of **columns** ($M$).

```text
               Col 0   Col 1   Col 2
Row 0 --->  [ [ 1  ], [ 2  ], [ 3  ] ]
Row 1 --->  [ [ 4  ], [ 5  ], [ 6  ] ]
Row 2 --->  [ [ 7  ], [ 8  ], [ 9  ] ]
```

Element at `matrix[1][2]` is `6`.

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Matrix** | A two-dimensional rectangular array of numbers organized in $N$ rows and $M$ columns. |
| **Row Index ($i$)** | Zero-based index representing vertical position ($0 \le i < N$). |
| **Column Index ($j$)** | Zero-based index representing horizontal position ($0 \le j < M$). |
| **Primary Diagonal** | Top-left to bottom-right diagonal elements where $i == j$. |
| **Secondary Diagonal** | Top-right to bottom-left diagonal elements where $i + j == N - 1$. |
| **Staircase Search** | An $O(N + M)$ search algorithm starting at matrix corners for row-and-column-sorted matrices. |
| **Spiral Traversal** | Traversing grid boundaries in clockwise loops (Top $\to$ Right $\to$ Bottom $\to$ Left). |

---

## 4. Visual / Mental Model

### Matrix Diagonals ($3 \times 3$)

```text
  [ (0,0)   (0,1)   (0,2) ]     Primary Diagonal (PD): (0,0), (1,1), (2,2) -> i == j
  [ (1,0)   (1,1)   (1,2) ]     Secondary Diagonal (SD): (0,2), (1,1), (2,0) -> i + j == N - 1
  [ (2,0)   (2,1)   (2,2) ]     Center Element (1,1) belongs to BOTH! Do not double-count!
```

### Spiral Matrix Boundary Traversal

```text
    top ───►  1  ───►  2  ───►  3  ───►  4
                                         │
              12  ───► 13 ────► 14       ▼  right
              ▲                  │       5
              │       16 ◄── 15  │       │
              ▲                  ▼       ▼
    bottom ── 11 ◄─── 10 ◄──── 9 ◄──── 8
              ▲
             left
```

---

## 5. Operations / Techniques

### 1. Matrix Access & Traversal
- Nested `for` loops iterate through every cell in $O(N \times M)$ time.

### 2. Diagonal Sum Optimization
- **Naive approach:** Double loop checking $i==j$ or $i+j==N-1$ in $O(N^2)$ time.
- **Optimal approach:** Single loop $i$ from $0$ to $N-1$:
  - Add primary element `matrix[i][i]`.
  - If $i \neq N - 1 - i$, add secondary element `matrix[i][N - 1 - i]`.
  - Runs in $O(N)$ time!

### 3. Staircase Search in Row & Column Sorted Matrix
- Start search at Top-Right `(0, M-1)` or Bottom-Left `(N-1, 0)`.
- If `cell == key`: Found! Return `true`.
- If `cell > key`: Move **Left** (`col--`).
- If `cell < key`: Move **Down** (`row++`).
- Eliminates one row or one column per step $\implies O(N + M)$ time complexity.

---

## 6. Worked Examples

### Worked Example 1: Staircase Search

**Matrix ($4 \times 4$ sorted rows & cols), Key = 33:**

```text
[ 10, 20, 30, 40 ]
[ 15, 25, 35, 45 ]
[ 27, 29, 37, 48 ]
[ 32, 33, 39, 50 ]
```

- **Step 1:** Start at Top-Right `row = 0, col = 3` (`val = 40`).
  - $40 > 33 \implies$ Key is smaller! Move **Left** (`col = 2`).
- **Step 2:** Position `row = 0, col = 2` (`val = 30`).
  - $30 < 33 \implies$ Key is larger! Move **Down** (`row = 1`).
- **Step 3:** Position `row = 1, col = 2` (`val = 35`).
  - $35 > 33 \implies$ Key is smaller! Move **Left** (`col = 1`).
- **Step 4:** Position `row = 1, col = 1` (`val = 25`).
  - $25 < 33 \implies$ Key is larger! Move **Down** (`row = 2`).
- **Step 5:** Position `row = 2, col = 1` (`val = 29`).
  - $29 < 33 \implies$ Key is larger! Move **Down** (`row = 3`).
- **Step 6:** Position `row = 3, col = 1` (`val = 33`).
  - $33 == 33 \implies$ **Found key at `(3, 1)`!**

---

## 7. Java Implementation Concepts

- **Declaration & Allocation:**
  ```java
  int[][] matrix = new int[3][4]; // 3 rows, 4 columns initialized to 0
  int[][] grid = {
      {1, 2, 3},
      {4, 5, 6},
      {7, 8, 9}
  };
  ```
- **Jagged Arrays:** Java allows rows of unequal length (e.g. `grid[0] = new int[2]; grid[1] = new int[5];`). Always use `grid[i].length` when looping over columns of row `i`.
- **Memory Structure:** `grid` is an array of references to 1D integer arrays stored across heap space.

---

## 8. Problem-Solving Patterns

### Pattern 1: Boundary Movement (Spiral Traversal)
- **When to think of it:** Processing outer layers of a matrix inward.
- **Mental Approach:** Maintain 4 boundary pointers: `startRow`, `endRow`, `startCol`, `endCol`. Shrink boundaries inward after processing each side.

### Pattern 2: Staircase Reduction
- **When to think of it:** Matrix sorted both horizontally (rows) and vertically (columns).
- **Mental Approach:** Choose corner with opposing sorting trends (e.g., Top-Right: moving left decreases, moving down increases). Eliminate one dimension per decision.

---

## 9. Algorithms

### Staircase Search Algorithm
- **Problem solved:** Find key in $N \times M$ matrix where rows and columns are sorted.
- **Pseudocode:**
  ```text
  function staircaseSearch(matrix, key):
      row = 0, col = matrix[0].length - 1
      while row < matrix.length and col >= 0:
          if matrix[row][col] == key return true
          else if matrix[row][col] > key col--
          else row++
      return false
  ```
- **Time Complexity:** $O(N + M)$ | **Space Complexity:** $O(1)$

---

## 10. Complexity Reference

| Operation / Problem | Time Complexity | Space Complexity |
|---|---|---|
| Matrix Cell Access (`matrix[r][c]`) | $O(1)$ | $O(1)$ |
| Full Grid Traversal | $O(N \times M)$ | $O(1)$ |
| Naive Diagonal Sum | $O(N^2)$ | $O(1)$ |
| Optimal Diagonal Sum | $O(N)$ | $O(1)$ |
| Spiral Matrix Traversal | $O(N \times M)$ | $O(1)$ extra |
| Brute Force Search | $O(N \times M)$ | $O(1)$ |
| Staircase Search | $O(N + M)$ | $O(1)$ |

---

## 11. Common Mistakes

- **Swapping Row and Column Indices:** Writing `matrix[col][row]` instead of `matrix[row][col]`, leading to `ArrayIndexOutOfBoundsException` in non-square matrices ($N \neq M$).
- **Double Counting Center Element in Diagonal Sum:** Adding the center cell twice when $N$ is odd ($N = M$).
- **Spiral Loop Edge Case Overlapping:** Printing duplicate elements on the last inner row/col when `startRow == endRow` or `startCol == endCol`. Ensure `if (startRow == endRow) break;` checks are in place.

---

## 12. Edge Cases

- **$1 \times 1$ Matrix:** Single cell grid.
- **Single Row Matrix ($1 \times M$):** $N = 1$.
- **Single Column Matrix ($N \times 1$):** $M = 1$.
- **Non-Square Rectangular Matrix ($N \neq M$).**
- **Key Not Present in Matrix.**

---

## 13. Interview Questions

### Beginner
1. How are 2D arrays stored in Java heap memory?
2. Write a program to print a matrix in row-major order.

### Intermediate
1. Explain how to compute the diagonal sum of an $N \times N$ matrix in $O(N)$ time instead of $O(N^2)$.
2. Trace Spiral Traversal on a $3 \times 4$ non-square matrix.

### Advanced
1. Prove why Staircase Search achieves $O(N + M)$ time complexity on row-and-column-sorted matrices.
2. How do you rotate an $N \times N$ matrix by 90 degrees in-place? (Transpose + Reverse Rows).

---

## 14. Real-World Applications

- **Computer Graphics & Game Grids:** Storing map terrain, chessboards, pixel color buffers.
- **Spreadsheets & Data Frames:** Tables in Excel / SQL stored as rows and columns.
- **Dynamic Programming Tables:** 2D grid DP tables for shortest paths and knapsack problems.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`MatrixBasics.java`](MatrixBasics.java) | 2D matrix initialization, input/output scanning, and grid traversal. |
| [`DiagonalSum.java`](DiagonalSum.java) | Computing primary and secondary diagonal sums in $O(N)$ linear time without double-counting center. |
| [`SearchInSortedMatrix.java`](SearchInSortedMatrix.java) | Staircase Search algorithm searching key in $O(N+M)$ time starting at top-right corner. |
| [`SpiralMatrix.java`](SpiralMatrix.java) | Boundary-based spiral order traversal printing matrix layer by layer clockwise. |

---

## 16. Related Topics

### Prerequisites
- 1D Arrays & Nested Loops.

### Related Topics
- Searching Algorithms.
- Grid-based Backtracking (Maze solving, N-Queens).

### Next Topics
- Sorting Algorithms (Bubble, Selection, Counting).
- Dynamic Programming on Grids.

---

# Chromium 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for Matrix Spiral Traversal or Staircase Search paths here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
