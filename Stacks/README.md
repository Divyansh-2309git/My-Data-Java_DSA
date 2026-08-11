# Stacks & Monotonic Stack Patterns

## 1. What Is This?

A Stack is a linear data structure that operates according to the **Last-In, First-Out (LIFO)** principle. The last element added (pushed) onto the stack is the first element removed (popped).

Think of a stack of cafeteria trays: you place new trays on top, and you pick trays off the top.

Stacks are fundamental to computer science—used in compiler call stacks, expression evaluation (Infix to Postfix), undo operations in text editors, and advanced algorithmic patterns like **Monotonic Stacks** for linear-time range queries.

---

## 2. Core Idea

All operations occur exclusively at one end of the container called the **TOP**:

```text
               TOP
                │
                ▼
             [ 30 ]  <-- Push / Pop / Peek here!
             [ 20 ]
             [ 10 ]
            └──────┘
```

1. **Push:** Add an element onto the top of the stack ($O(1)$).
2. **Pop:** Remove and return the top element of the stack ($O(1)$).
3. **Peek:** View the top element without removing it ($O(1)$).
4. **IsEmpty:** Check if the stack contains zero elements ($O(1)$).

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **LIFO** | Last-In, First-Out operational order. |
| **Top** | Pointer / index referring to the most recently inserted element. |
| **Monotonic Stack** | A stack whose elements are strictly kept in increasing or decreasing order. Used to find next greater/smaller elements in $O(N)$ time. |
| **Stock Span** | Maximum number of consecutive days prior to today where stock price was $\le$ today's price. |
| **Max Area Histogram** | Finding the maximum rectangular area formed by contiguous height bars in a histogram. |

---

## 4. Visual / Mental Model

### Monotonic Stack for Next Greater Element

Array: `[6, 8, 0, 1, 3]`, Scanning Right-to-Left:

```text
  Element i=4 (val=3): Stack is empty. NextGreater[4] = -1. Push 3.  Stack: [3]
  Element i=3 (val=1): Top is 3 (> 1). NextGreater[3] = 3.  Push 1.  Stack: [3, 1]
  Element i=2 (val=0): Top is 1 (> 0). NextGreater[2] = 1.  Push 0.  Stack: [3, 1, 0]
  Element i=1 (val=8): Pop 0, Pop 1, Pop 3 (all <= 8). Stack empty! NextGreater[1] = -1. Push 8. Stack: [8]
  Element i=0 (val=6): Top is 8 (> 6). NextGreater[0] = 8.  Push 6.  Stack: [8, 6]

  Result Array: [8, -1, 1, 3, -1]  Computed in single pass O(N) time!
```

---

## 5. Operations / Techniques

### Implementations Comparison

1. **ArrayList Implementation:** Top is `list.size() - 1`. Fast random access, amortized $O(1)$ push.
2. **Linked List Implementation:** Top is `head`. Clean $O(1)$ push (`addFirst`) and pop (`removeFirst`).
3. **Java Standard Library:**
   - `java.util.Stack` (Legacy class, inherits from `Vector`, synchronized).
   - `java.util.ArrayDeque` (Recommended modern Java stack implementation: `Deque<Integer> stack = new ArrayDeque<>()`).

---

## 6. Worked Examples

### Worked Example: Max Area in Histogram

**Histogram Heights:** `[2, 1, 5, 6, 2, 3]`

- **Step 1:** Compute Next Smaller Right (NSR) boundary indices: `[1, 6, 4, 4, 6, 6]`.
- **Step 2:** Compute Next Smaller Left (NSL) boundary indices: `[-1, -1, 1, 2, 1, 4]`.
- **Step 3:** Compute Area for each bar $i$:
  $$\text{Width}[i] = \text{NSR}[i] - \text{NSL}[i] - 1$$
  $$\text{Area}[i] = \text{Height}[i] \times \text{Width}[i]$$
  - $i=0 (h=2): \text{width} = 1 - (-1) - 1 = 1 \implies \text{Area} = 2 \times 1 = 2$
  - $i=1 (h=1): \text{width} = 6 - (-1) - 1 = 6 \implies \text{Area} = 1 \times 6 = 6$
  - $i=2 (h=5): \text{width} = 4 - 1 - 1 = 2 \implies \text{Area} = 5 \times 2 = 10$
  - $i=3 (h=6): \text{width} = 4 - 2 - 1 = 1 \implies \text{Area} = 6 \times 1 = 6$
  - $i=4 (h=2): \text{width} = 6 - 1 - 1 = 4 \implies \text{Area} = 2 \times 4 = 8$
  - $i=5 (h=3): \text{width} = 6 - 4 - 1 = 1 \implies \text{Area} = 3 \times 1 = 3$
- **Result:** Maximum Area is **`10`** (formed by bar height 5 and bar height 6 across width 2).

---

## 7. Java Implementation Concepts

- **Legacy `Stack` vs `ArrayDeque`:**
  ```java
  // Legacy (Slower due to thread synchronization locks)
  Stack<Integer> stack = new Stack<>();

  // Recommended Standard Java Deque Stack (Faster & Unsynchronized)
  Deque<Integer> stack = new ArrayDeque<>();
  stack.push(10);
  int val = stack.pop();
  ```
- **Recursive Stack Manipulation:** Reversing a stack using recursion avoids $O(N)$ auxiliary array space by leveraging the JVM Call Stack frames!

---

## 8. Problem-Solving Patterns

### Pattern 1: Monotonic Stack (Next Greater / Smaller Element)
- **When to think of it:** Finding the nearest element to the left/right that is greater/smaller than current element.
- **Mental Approach:** Maintain stack in monotonic decreasing order. While `stack.peek() <= current`, pop elements!

### Pattern 2: Matching Bracket Pairs
- **When to think of it:** Valid Parentheses `({[]})`, Duplicate Parentheses, HTML/XML tag matching.
- **Mental Approach:** Push opening brackets `(`, `{`, `[`. When encountering closing bracket, verify `stack.peek()` matches opening bracket type and pop.

---

## 9. Algorithms

### Next Greater Element Algorithm (Monotonic Stack)
- **Pseudocode:**
  ```text
  function nextGreaterElement(arr):
      n = arr.length
      nxtGreater = new int[n]
      stack = new Stack()
      for i = n - 1 down to 0:
          while not stack.isEmpty() and arr[stack.peek()] <= arr[i]:
              stack.pop()
          nxtGreater[i] = stack.isEmpty() ? -1 : arr[stack.peek()]
          stack.push(i)
      return nxtGreater
  ```
- **Time Complexity:** $O(N)$ (Each element pushed and popped at most once!) | **Space:** $O(N)$

---

## 10. Complexity Reference

| Operation / Problem | Time Complexity | Space Complexity |
|---|---|---|
| Push | $O(1)$ | $O(1)$ |
| Pop | $O(1)$ | $O(1)$ |
| Peek | $O(1)$ | $O(1)$ |
| Push at Bottom (Rec) | $O(N)$ | $O(N)$ call stack |
| Reverse Stack (Rec) | $O(N^2)$ | $O(N)$ call stack |
| Valid Parentheses Check | $O(N)$ | $O(N)$ |
| Stock Span Problem | $O(N)$ | $O(N)$ |
| Next Greater Element | $O(N)$ | $O(N)$ |
| Max Area Histogram | $O(N)$ | $O(N)$ |

---

## 11. Common Mistakes

- **Calling `pop()` or `peek()` on Empty Stack:** Throws `EmptyStackException` or `NoSuchElementException`. Always guard with `while (!stack.isEmpty())`.
- **Using `java.util.Stack` in Performance Code:** `java.util.Stack` extends legacy `Vector` with thread-locking overhead. Use `ArrayDeque` instead.
- **Nested Loops in Monotonic Stack:** Assuming `while` loop inside `for` loop makes Next Greater Element $O(N^2)$. It is **$O(N)$** because every index is pushed and popped at most once across the entire algorithm execution!

---

## 12. Edge Cases

- **Empty Input / Empty String.**
- **All Opening Brackets (`"((((("`).**
- **All Closing Brackets (`")))))"`).**
- **Strictly Increasing / Strictly Decreasing Arrays.**

---

## 13. Interview Questions

### Beginner
1. What is the LIFO principle, and how does a Stack differ from a Queue?
2. Compare Stack implementations using ArrayList vs Singly Linked List.

### Intermediate
1. Explain how to check for Valid Parentheses using a Stack.
2. How does the Monotonic Stack algorithm achieve $O(N)$ time complexity for the Next Greater Element problem?

### Advanced
1. Derive the $O(N)$ time complexity and boundary calculation logic for the Max Area Histogram problem.
2. Write a recursive function to reverse a Stack in-place without using any auxiliary loop or external data structures.

---

## 14. Real-World Applications

- **JVM Method Call Stack:** Tracking active thread function calls and local variables.
- **Browser Navigation:** Managing the "Back" button history.
- **Expression Evaluation:** Parsing Infix, Postfix, and Prefix mathematical expressions in compilers.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`StackArrayListImplementation.java`](StackArrayListImplementation.java) | Custom Stack data structure built using `ArrayList` dynamic storage. |
| [`StackLinkedListImplementation.java`](StackLinkedListImplementation.java) | Custom Stack data structure built using Node-based `LinkedList` ($O(1)$ operations). |
| [`StackJCFDemo.java`](StackJCFDemo.java) | Using Java Collections Framework standard `java.util.Stack`. |
| [`PushAtStackBottom.java`](PushAtStackBottom.java) | Inserting an element at the bottom of a stack using call stack recursion ($O(N)$ time). |
| [`ReverseStringUsingStack.java`](ReverseStringUsingStack.java) | Reversing character sequence by pushing chars and popping sequentially. |
| [`ReverseStackRecursive.java`](ReverseStackRecursive.java) | In-place recursive reversal of a stack utilizing `pushAtBottom`. |
| [`StockSpanProblem.java`](StockSpanProblem.java) | Monotonic stack algorithm calculating stock span days in $O(N)$ time. |
| [`NextGreaterElement.java`](NextGreaterElement.java) | Classical Monotonic Stack algorithm finding nearest greater right element in $O(N)$ time. |
| [`ValidParenthesesCheck.java`](ValidParenthesesCheck.java) | Matching opening and closing brackets (`()`, `{}`, `[]`) using Stack. |
| [`DuplicateParenthesesCheck.java`](DuplicateParenthesesCheck.java) | Detecting redundant parentheses pairs in algebraic expressions. |
| [`MaxAreaHistogram.java`](MaxAreaHistogram.java) | Optimal $O(N)$ Monotonic Stack algorithm computing maximum rectangular area in a histogram grid. |

---

## 16. Related Topics

### Prerequisites
- Arrays, ArrayList, and Singly Linked Lists.

### Related Topics
- Queues & Deque (FIFO structures).
- Expression Parsing (Infix / Postfix).

### Next Topics
- Queues & Double-Ended Queues (Deque).
- Monotonic Queue & Sliding Window Maximum.

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for Monotonic Stack step trace or Max Area Histogram boundaries here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
