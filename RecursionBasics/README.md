# Recursion Fundamentals

## 1. What Is This?

Recursion is a programming technique where a method calls itself to solve a smaller instance of the exact same problem.

Recursion breaks complex multi-step problems down into simple sub-problems. It is the primary engine behind Divide & Conquer (Merge Sort, Quick Sort), Backtracking (N-Queens, Subsets), Tree Traversals (Preorder, Inorder, Postorder), Graph Depth-First Search (DFS), and Dynamic Programming.

---

## 2. Core Idea

Every recursive function MUST have two primary components:

1. **Base Case:** The condition under which the function stops calling itself and returns a direct answer. *Without a base case, recursion leads to infinite execution and `StackOverflowError`!*
2. **Recursive Case:** The line where the function calls itself with a reduced/smaller argument (e.g. $N-1$ or $N/2$), moving closer to the base case.

```text
               ┌─────────────────────────────┐
               │    fn(N) Call               │
               └──────────────┬──────────────┘
                              │  N > Base
                              ▼
               ┌─────────────────────────────┐
               │  Work + fn(N - 1) Call      │
               └──────────────┬──────────────┘
                              │  Reaches Base Case
                              ▼
               ┌─────────────────────────────┐
               │  Base Case Reached -> Return│
               └─────────────────────────────┘
```

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Base Case** | The termination condition that stops recursion. |
| **Recursive Step** | The self-invocation step operating on smaller sub-problems. |
| **Call Stack** | Memory stack that holds active function stack frames during recursive depth expansion. |
| **Stack Overflow** | Error (`StackOverflowError`) thrown when call stack space is exhausted due to missing base case. |
| **Recursion Tree** | A tree diagram depicting method calls and branching at each level of recursion. |
| **Tail Recursion** | A recursive call that occurs as the absolute final statement of the function. |

---

## 4. Visual / Mental Model

### Call Stack Execution Trace for `factorial(4)`

```text
  factorial(4) = 4 * factorial(3)  --->  Waiting for factorial(3)...
    factorial(3) = 3 * factorial(2)  --->  Waiting for factorial(2)...
      factorial(2) = 2 * factorial(1)  --->  Waiting for factorial(1)...
        factorial(1) = 1 * factorial(0)  --->  Waiting for factorial(0)...
          factorial(0) = 1 (BASE CASE REALSES!)

  [ UNWINDING / STACK POPS ]
  factorial(1) returns 1 * 1 = 1
  factorial(2) returns 2 * 1 = 2
  factorial(3) returns 3 * 2 = 6
  factorial(4) returns 4 * 6 = 24  ===> FINAL RESULT
```

### Recursion Tree for Fibonacci `fib(4)`

```text
                        fib(4)
                     /          \
                fib(3)          fib(2)
               /      \        /      \
           fib(2)    fib(1)  fib(1)  fib(0)
          /      \
      fib(1)    fib(0)
```

Notice duplicate computations! `fib(2)` is computed twice. Total nodes $\approx 2^N \implies O(2^N)$ time complexity!

---

## 5. Operations / Techniques

### 1. Head Recursion vs Tail Recursion
- **Head Recursion:** Recursive call happens *before* processing work (e.g. printing numbers 1 to N).
- **Tail Recursion:** Recursive call happens *after* processing work (e.g. printing numbers N to 1).

### 2. Friends Pairing Problem
- $N$ friends want to go to a party. Each can go single or paired up with another friend.
- If $N$-th friend goes **single**: Remaining friends $= f(n-1)$.
- If $N$-th friend pairs up with any of $(n-1)$ choices: Remaining friends $= (n-1) \times f(n-2)$.
- **Recurrence Formula:** $f(n) = f(n-1) + (n-1) \times f(n-2)$

### 3. Binary Strings Without Consecutive Ones
- Count binary strings of length $N$ with no consecutive `1`s.
- Place `0` at position $i$: Remaining choices $= f(n-1)$.
- Place `1` at position $i$: Next position MUST be `0`, so remaining choices $= f(n-2)$.
- **Recurrence Formula:** $f(n) = f(n-1) + f(n-2)$ (Fibonacci structure!).

---

## 6. Worked Examples

### Worked Example: Binary Strings of Length $N=3$ (No Consecutive 1s)

- **Input:** $N = 3$, `lastPlace = 0`
- **Trace:**
  - $N=3$: Option '0' $\to$ call $(N=2, \text{last}=0)$. Option '1' $\to$ call $(N=2, \text{last}=1)$.
  - From $(N=2, \text{last}=0)$: Can spawn $(N=1, \text{last}=0)$ and $(N=1, \text{last}=1)$.
  - From $(N=2, \text{last}=1)$: Can ONLY spawn $(N=1, \text{last}=0)$ because last was 1!
- **Base Cases ($N=0$):** Print accumulated string:
  - `"000"`, `"001"`, `"010"`, `"100"`, `"101"`.
- **Result:** **5 valid binary strings** out of $2^3 = 8$ total combinations.

---

## 7. Java Implementation Concepts

- **Stack Overflow Exception:** `java.lang.StackOverflowError` occurs when recursion depth exceeds default stack capacity (typically $\approx 10,000$ frames).
- **Recursion vs Iteration:**
  - Iteration uses explicit loops (`for`, `while`) with $O(1)$ stack space.
  - Recursion uses internal Call Stack frames, consuming $O(N)$ auxiliary stack memory.

---

## 8. Problem-Solving Patterns

### Pattern 1: Mathematical Recurrence Relation
- **When to think of it:** Factorial, Fibonacci, Tiling, Friends pairing.
- **Mental Approach:** Express solution for $N$ in terms of solutions for $N-1$ and $N-2$. Identify base cases ($N=0$ or $N=1$).

### Pattern 2: Choice-Based Accumulator
- **When to think of it:** String manipulations, subset generation, binary string generation.
- **Mental Approach:** Pass `String ans` or `int index` as method arguments. Spawn multiple recursive branches for each valid decision choice.

---

## 9. Algorithms

### Friends Pairing Recursive Algorithm
- **Pseudocode:**
  ```text
  function friendsPairing(n):
      if n == 1 or n == 2: return n
      // Choice 1: Single + Choice 2: Paired
      return friendsPairing(n - 1) + (n - 1) * friendsPairing(n - 2)
  ```
- **Time Complexity:** $O(2^N)$ without Memoization | **Space:** $O(N)$ stack depth

---

## 10. Complexity Reference

| Problem / Algorithm | Time Complexity | Auxiliary Space (Call Stack) |
|---|---|---|
| Factorial ($N$) | $O(N)$ | $O(N)$ |
| Fibonacci Naive | $O(2^N)$ | $O(N)$ |
| Print Numbers (1 to $N$) | $O(N)$ | $O(N)$ |
| Check Array Sorted | $O(N)$ | $O(N)$ |
| First / Last Occurrence | $O(N)$ | $O(N)$ |
| Remove Duplicates | $O(N)$ | $O(N)$ |
| Friends Pairing | $O(2^N)$ | $O(N)$ |
| Binary Strings (No 11s) | $O(2^N)$ | $O(N)$ |

---

## 11. Common Mistakes

- **Missing / Incorrect Base Case:** Causing infinite recursive execution and instant `StackOverflowError`.
- **Incorrect Variable Mutation:** Modifying global variables instead of passing updated parameters (`n-1`) into the recursive call. Writing `f(n--)` passes $n$ first (infinite loop!) instead of `f(n-1)`.
- **Duplicate Computation:** Computing overlapping subproblems repeatedly in naive Fibonacci ($O(2^N)$ time explosion).

---

## 12. Edge Cases

- **Base Inputs:** $N = 0$ or $N = 1$.
- **Negative Inputs:** Throwing `IllegalArgumentException` for invalid negative factorial or count inputs.
- **Large Stack Depths:** $N > 10,000$ causing stack overflow.

---

## 13. Interview Questions

### Beginner
1. What is a Base Case, and why is it mandatory in every recursive function?
2. Trace the call stack during the execution of `factorial(3)`.

### Intermediate
1. Explain why naive recursive Fibonacci has exponential $O(2^N)$ time complexity.
2. Differentiate between Head Recursion and Tail Recursion.

### Advanced
1. Derive the recurrence relation for the Friends Pairing Problem: $f(n) = f(n-1) + (n-1)f(n-2)$.
2. Explain how Binary Strings without consecutive 1s maps directly to the Fibonacci recurrence sequence.

---

## 14. Real-World Applications

- **File System Tree Traversal:** Navigating nested directories and files on hard drives.
- **JSON / HTML DOM Parsers:** Parsing nested data elements.
- **Compiler Parsing:** Abstract Syntax Tree (AST) evaluation.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`BinaryStringsNoConsecutiveOnes.java`](BinaryStringsNoConsecutiveOnes.java) | Recursive generation of binary strings of length $N$ without consecutive 1s. |
| [`FirstOccurrence.java`](FirstOccurrence.java) | Finding first index of key in an array using forward recursion. |
| [`FriendsPairingProblem.java`](FriendsPairingProblem.java) | Combinatorial recursive problem counting ways $N$ friends can remain single or pair up. |
| [`IsArraySortedRecursive.java`](IsArraySortedRecursive.java) | Checking if array is strictly sorted in ascending order using recursion. |
| [`LastOccurrence.java`](LastOccurrence.java) | Finding last occurrence index of key in an array using recursive unwinding. |
| [`PrintDecreasingNumbers.java`](PrintDecreasingNumbers.java) | Tail recursive printing of numbers from $N$ down to 1. |
| [`PrintIncreasingNumbers.java`](PrintIncreasingNumbers.java) | Head recursive printing of numbers from 1 up to $N$. |
| [`RecursiveFactorial.java`](RecursiveFactorial.java) | Classic factorial calculation ($N! = N \times (N-1)!$) with base case $N=0$. |
| [`RecursiveFibonacci.java`](RecursiveFibonacci.java) | Fibonacci number computation using dual recursive calls ($f(n-1) + f(n-2)$). |
| [`RemoveDuplicatesString.java`](RemoveDuplicatesString.java) | Recursive removal of duplicate characters from string using boolean tracking map. |
| [`SumNaturalNumbers.java`](SumNaturalNumbers.java) | Calculating sum of first $N$ natural numbers recursively. |

---

## 16. Related Topics

### Prerequisites
- Java Basics & Functions.
- Call Stack Execution Frame Mechanics.

### Related Topics
- Divide & Conquer (Merge Sort).
- Backtracking & Tree Traversals.

### Next Topics
- Divide & Conquer Algorithms.
- Backtracking (Subsets, N-Queens).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for Call Stack frames or Fibonacci recursion tree here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
