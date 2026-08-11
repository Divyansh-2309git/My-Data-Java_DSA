# Heaps & Priority Queues

## 1. What Is This?

A Heap is a specialized tree-based data structure that satisfies two defining characteristics:

1. **Complete Binary Tree Structure:** Every level of the tree is completely filled except possibly the last level, which is filled from left to right.
2. **Heap Property:**
   - **Min-Heap:** The value of every node is $\le$ the values of its children ($\text{Root} = \text{Global Minimum}$).
   - **Max-Heap:** The value of every node is $\ge$ the values of its children ($\text{Root} = \text{Global Maximum}$).

Heaps serve as the internal backbone for **Priority Queues**, enabling constant-time $O(1)$ retrieval of the highest-priority element and logarithmic $O(\log N)$ insertions and deletions.

---

## 2. Core Idea

Although a Heap is conceptually a Complete Binary Tree, it is physically stored in a continuous 1D Array or `ArrayList` without using any Node pointers!

For any element at index $i$:

```text
               10 (index 0)           Array representation:
              /  \                    Index:   0    1    2    3    4
            20    30                  Value: [ 10 ][ 20 ][ 30 ][ 40 ][ 50 ]
           /  \
          40  50 (idx 3, 4)
```

### Index Formulas (Refer to [notes.md](notes.md)):
- **Parent Index:** `(i - 1) / 2`
- **Left Child Index:** `2 * i + 1`
- **Right Child Index:** `2 * i + 2`

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **PriorityQueue** | Abstract Data Type where elements are processed based on priority order rather than FIFO/LIFO. |
| **Min-Heap** | Heap where the root holds the smallest element in the collection. |
| **Max-Heap** | Heap where the root holds the largest element in the collection. |
| **Up-Heapify (Up-Heap)** | Fixing heap property upward after insertion by repeatedly swapping with parent ($O(\log N)$). |
| **Down-Heapify (Heapify)**| Fixing heap property downward after root deletion by repeatedly swapping with smaller child ($O(\log N)$). |
| **Comparable** | Java interface (`compareTo()`) defining natural sorting order inside a class. |
| **Comparator** | External Java interface for custom comparison logic (e.g. `Collections.reverseOrder()`). |

---

## 4. Visual / Mental Model

### Min-Heap Up-Heapify Trace (Inserting 2)

Existing Heap: `[3, 4, 5, 10]`, Insert `2` at index 4 (last position):

```text
Step 1:  [ 3, 4, 5, 10, 2 ]   --> Parent of 2 (idx 4) is at (4-1)/2 = 1 (val 4).
                                  2 < 4 $\implies$ SWAP 2 and 4!

Step 2:  [ 3, 2, 5, 10, 4 ]   --> Parent of 2 (idx 1) is at (1-1)/2 = 0 (val 3).
                                  2 < 3 $\implies$ SWAP 2 and 3!

Step 3:  [ 2, 3, 5, 10, 4 ]   --> 2 reaches index 0 (ROOT). Up-heapify complete!
```

---

## 5. Operations / Techniques

### 1. Peek ($O(1)$)
Returns the root element (`list.get(0)`) without modifying the heap.

### 2. Insert / Add ($O(\log N)$)
1. Add new element to the end of the `ArrayList`.
2. Perform **Up-Heapify**: compare element with parent `(x-1)/2`. Swap while `child < parent` (Min-Heap).

### 3. Remove / Poll ($O(\log N)$)
1. Swap root element (`list.get(0)`) with the last element (`list.get(n-1)`).
2. Delete the last element from the list ($O(1)$).
3. Perform **Down-Heapify**: compare root with its left and right children. Swap with the smaller child while `parent > min(children)` (Min-Heap).

---

## 6. Worked Examples

### Worked Example: Down-Heapify Root Removal

**Min-Heap Array:** `[1, 3, 4, 10, 5]` (Remove Root `1`):

- **Step 1 (Swap & Remove):** Swap root `1` with last element `5` $\implies$ `[5, 3, 4, 10, 1]`. Remove `1` from end $\implies$ `[5, 3, 4, 10]`.
- **Step 2 (Down-Heapify at idx 0 = 5):**
  - Left child idx 1 (val 3), Right child idx 2 (val 4).
  - Minimum of children is `3` at idx 1.
  - $5 > 3 \implies$ Swap `5` and `3`. Array: `[3, 5, 4, 10]`.
- **Step 3 (Down-Heapify at idx 1 = 5):**
  - Left child idx 3 (val 10), Right child idx 4 (out of bounds).
  - $5 < 10 \implies$ No swap needed! Down-heapify stops.
- **Result:** **Valid Min-Heap: `[3, 5, 4, 10]`** (New root is `3`).

---

## 7. Java Implementation Concepts

- **Default PriorityQueue (Min-Heap):**
  ```java
  PriorityQueue<Integer> pq = new PriorityQueue<>(); // Default Min-Heap
  pq.add(10);
  pq.add(5);
  System.out.println(pq.peek()); // Outputs 5
  ```
- **Custom Max-Heap via Comparator:**
  ```java
  PriorityQueue<Integer> maxPq = new PriorityQueue<>(Comparator.reverseOrder());
  ```
- **Custom Objects with `Comparable`:**
  ```java
  static class Student implements Comparable<Student> {
      String name;
      int rank;
      public Student(String name, int rank) { this.name = name; this.rank = rank; }
      
      @Override
      public int compareTo(Student s2) {
          return this.rank - s2.rank; // Ascending order by rank
      }
  }
  ```

---

## 8. Problem-Solving Patterns

### Pattern 1: K-th Smallest / Largest Element
- **When to think of it:** Finding top K elements in an unsorted stream.
- **Mental Approach:** Use Min-Heap of size K for K largest elements (or Max-Heap of size K for K smallest elements).

### Pattern 2: Merge K Sorted Lists
- **When to think of it:** Combining $K$ sorted arrays/lists into a single sorted output.
- **Mental Approach:** Push first element of each list into Min-Heap. Extract min, then push next element from that list into heap.

---

## 9. Algorithms

### Down-Heapify Algorithm (Min-Heap)
- **Pseudocode:**
  ```text
  function heapify(i):
      left = 2 * i + 1
      right = 2 * i + 2
      minIdx = i
      
      if left < list.size() and list.get(left) < list.get(minIdx): minIdx = left
      if right < list.size() and list.get(right) < list.get(minIdx): minIdx = right
      
      if minIdx != i:
          swap(list, i, minIdx)
          heapify(minIdx)
  ```
- **Time Complexity:** $O(\log N)$ | **Space Complexity:** $O(\log N)$ call stack depth

---

## 10. Complexity Reference

| Operation / Problem | Time Complexity | Auxiliary Space |
|---|:---:|:---:|
| Peek Top (`peek()`) | $O(1)$ | $O(1)$ |
| Insert / Add (`add()`) | $O(\log N)$ | $O(1)$ |
| Remove / Poll (`poll()`) | $O(\log N)$ | $O(1)$ |
| Heapify Single Element | $O(\log N)$ | $O(\log N)$ |
| Build Heap from Array (`Heapify All`)| $O(N)$ (Linear math bound!) | $O(1)$ |
| Heap Sort | $O(N \log N)$ | $O(1)$ |

---

## 11. Common Mistakes

- **Incorrect ClassCastException:** Adding custom objects to `PriorityQueue` without implementing `Comparable<T>` or providing a `Comparator`.
- **Inverted Comparator Logic:** Returning `b - a` instead of `a - b` causing Min-Heap to behave like Max-Heap or integer subtraction underflow when values are negative.
- **Removing Arbitrary Elements ($O(N)$):** `pq.remove(val)` performs linear search $O(N)$ instead of logarithmic $O(\log N)$ heap removal.

---

## 12. Edge Cases

- **Empty Heap (`pq.peek() == null`).**
- **Single Element Heap.**
- **Duplicate Priorities / Values.**
- **Negative Integer Priorities.**

---

## 13. Interview Questions

### Beginner
1. What is a Complete Binary Tree, and how is it efficiently stored in a 1D Array?
2. Explain the difference between a Min-Heap and a Max-Heap.

### Intermediate
1. Explain the step-by-step process of inserting an element into a Min-Heap (Up-Heapify).
2. How do custom objects achieve priority ordering in Java's `PriorityQueue` using `Comparable` or `Comparator`?

### Advanced
1. Derive why building a Heap from an arbitrary array of size $N$ takes $O(N)$ time instead of $O(N \log N)$.
2. Compare Heap Sort ($O(N \log N)$ time, $O(1)$ space) with Merge Sort and Quick Sort.

---

## 14. Real-World Applications

- **Operating System Task Schedulers:** Priority-based process execution (e.g. Linux O(1) scheduler).
- **Graph Algorithms:** Dijkstra's Shortest Path and Prim's Minimum Spanning Tree algorithms.
- **Huffman Coding:** Data compression algorithms for ZIP files.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`HeapPeek.java`](HeapPeek.java) | $O(1)$ constant time top element retrieval in a Heap (`list.get(0)`). |
| [`MinHeapInsertion.java`](MinHeapInsertion.java) | Min-Heap Insertion algorithm using `ArrayList` and $O(\log N)$ Up-Heapify parent swapping. |
| [`RemoveFromHeap.java`](RemoveFromHeap.java) | Min-Heap Root Deletion algorithm using $O(\log N)$ Down-Heapify recursive downward swapping. |
| [`PriorityQueueDemo.java`](PriorityQueueDemo.java) | Standard Java Collections `PriorityQueue` usage for Min-Heap and Max-Heap using `Comparator.reverseOrder()`. |
| [`PriorityQueueCustomObjects.java`](PriorityQueueCustomObjects.java) | Priority Queue ordering for custom Java objects implementing the `Comparable` interface. |
| [`notes.md`](notes.md) | Dedicated formula note for array index relationships in complete binary trees. |

---

## 16. Related Topics

### Prerequisites
- Arrays, ArrayLists, and Binary Trees.

### Related Topics
- Sorting Algorithms (Heap Sort).
- Graph Algorithms (Dijkstra & Prim).

### Next Topics
- Hashing (HashMap & HashSet).
- Advanced Graph Algorithms.

---

# 🖼️ 17. Visual Notes / Personal Images

## Personal Visual Notes & Formulas

The repository contains visual notes for Heap indexing:

![Heap Array Index Formulas Note](image.png)

### What the image represents
- **Complete Binary Tree Array Mapping:** Formula equations showing parent `(i-1)/2`, left child `2i+1`, and right child `2i+2`.
- **Heap Representation:** Step-by-step node to array index correspondence. For dedicated notes, see [notes.md](notes.md).
