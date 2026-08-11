# Queues & Deque Data Structures

## 1. What Is This?

A Queue is a linear data structure that operates according to the **First-In, First-Out (FIFO)** principle. The first element added to the queue is the very first element removed.

Think of a real-world line of people waiting for tickets: the person who enters the queue first gets served first.

Queues are essential in operating systems (CPU process scheduling, IO buffers), network packet processing, Breadth-First Search (BFS) graph traversals, and asynchronous message queues (RabbitMQ, Kafka).

---

## 2. Core Idea

Elements enter at the **REAR** (enqueue) and exit at the **FRONT** (dequeue):

```text
       dequeue / remove                          enqueue / add
           FRONT                                    REAR
             │                                       │
             ▼                                       ▼
          [ 10 ]  ──►  [ 20 ]  ──►  [ 30 ]  ──►   [ 40 ]
```

1. **Enqueue / Add:** Insert element at the rear ($O(1)$).
2. **Dequeue / Remove:** Extract element from the front ($O(1)$).
3. **Peek / Element:** View front element without removing ($O(1)$).

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **FIFO** | First-In, First-Out operational order. |
| **Front** | Pointer/reference tracking the head element to be dequeued. |
| **Rear** | Pointer/reference tracking the tail element where new items are enqueued. |
| **Circular Queue** | Array implementation using modular arithmetic (`(rear + 1) % size`) to reuse freed front slots. |
| **Deque (Double-Ended Queue)**| Data structure allowing insertion and deletion at BOTH front and rear ends ($O(1)$). |

---

## 4. Visual / Mental Model

### Circular Queue Array Mechanics

Standard array queues waste freed front memory after dequeuing. A **Circular Queue** wraps pointers around using modulo `% capacity`:

```text
Capacity = 5. Array: [ 40 , 50 ,    , 20 , 30 ]
                       │     │        │    │
                       │     ▼        ▼    │
                     rear   (free)  front  │
```

- Enqueue index calculation: `rear = (rear + 1) % capacity`
- Dequeue index calculation: `front = (front + 1) % capacity`

---

## 5. Operations / Techniques

### Implementations Comparison

| Implementation | Enqueue Time | Dequeue Time | Space Complexity |
|---|:---:|:---:|:---:|
| **Fixed Array (Naive)** | $O(1)$ | $O(N)$ (requires shifting) | $O(N)$ |
| **Circular Array** | $O(1)$ | $O(1)$ | $O(N)$ |
| **Linked List** | $O(1)$ | $O(1)$ | $O(N)$ |
| **Queue using 2 Stacks** | $O(1)$ amortized | $O(1)$ amortized | $O(N)$ |
| **Deque (`ArrayDeque`)** | $O(1)$ | $O(1)$ | $O(N)$ |

---

## 6. Worked Examples

### Worked Example 1: First Non-Repeating Character in Stream

**Input Stream:** `"a a b c c"`

- **Step 1:** Read `'a'`. Frequency `freq['a'] = 1`. Add `'a'` to queue `[a]`. Queue front `'a'` has freq 1 $\implies$ **Output: `'a'`**.
- **Step 2:** Read `'a'`. Frequency `freq['a'] = 2`. Add `'a'` to queue `[a, a]`.
  - Check queue front: `freq['a'] = 2 > 1` $\implies$ Pop `'a'`.
  - Queue becomes empty $\implies$ **Output: `-1`**.
- **Step 3:** Read `'b'`. Frequency `freq['b'] = 1`. Add `'b'` to queue `[b]`. Queue front `'b'` has freq 1 $\implies$ **Output: `'b'`**.
- **Step 4:** Read `'c'`. Frequency `freq['c'] = 1`. Add `'c'` to queue `[b, c]`. Queue front `'b'` has freq 1 $\implies$ **Output: `'b'`**.
- **Step 5:** Read `'c'`. Frequency `freq['c'] = 2`. Add `'c'` to queue `[b, c, c]`.
  - Queue front `'b'` still has freq 1 $\implies$ **Output: `'b'`**.
- **Final Output Stream:** `a -1 b b b`

### Worked Example 2: Interleave Queue Halves

**Input Queue:** `[1, 2, 3, 4, 5, 6, 7, 8]` (Size $N=8$, Half size $N/2=4$)

- **Step 1:** Extract first half into auxiliary `firstHalf` queue: `[1, 2, 3, 4]`. Original queue contains second half `[5, 6, 7, 8]`.
- **Step 2:** Alternately dequeue from `firstHalf` and original queue, appending into result:
  - Take `1` from `firstHalf`, take `5` from original $\implies$ `[1, 5]`
  - Take `2` from `firstHalf`, take `6` from original $\implies$ `[1, 5, 2, 6]`
  - Take `3` from `firstHalf`, take `7` from original $\implies$ `[1, 5, 2, 6, 3, 7]`
  - Take `4` from `firstHalf`, take `8` from original $\implies$ `[1, 5, 2, 6, 3, 7, 4, 8]`
- **Result:** **`[1, 5, 2, 6, 3, 7, 4, 8]`**

---

## 7. Java Implementation Concepts

- **Queue Interface in Java:**
  ```java
  // In Java Collections Framework, Queue is an Interface instantiated via LinkedList or ArrayDeque:
  Queue<Integer> q = new LinkedList<>();
  q.add(10);     // Throws exception if full
  q.offer(20);   // Returns false if full (Preferred!)
  int val = q.poll(); // Returns null if empty (Preferred over remove!)
  ```
- **Deque Interface:**
  ```java
  Deque<Integer> deque = new ArrayDeque<>();
  deque.addFirst(1);
  deque.addLast(2);
  deque.removeFirst();
  deque.removeLast();
  ```

---

## 8. Problem-Solving Patterns

### Pattern 1: Frequency Array + Queue Stream Processing
- **When to think of it:** Finding first non-repeating character in a real-time data stream.
- **Mental Approach:** Use integer array `freq[26]` for count tracking, and `Queue<Character>` to preserve exact character arrival order.

### Pattern 2: Queue Reversal
- **When to think of it:** Reversing elements in a queue.
- **Mental Approach:** Dequeue all elements into a `Stack` (LIFO inversion), then pop stack elements back into Queue.

---

## 9. Algorithms

### Queue Using Two Stacks
- **Pseudocode:**
  ```text
  class QueueUsingTwoStacks:
      s1 = new Stack(), s2 = new Stack()
      
      function push(x): // O(1)
          s1.push(x)

      function pop(): // Amortized O(1)
          if s2.isEmpty():
              while not s1.isEmpty(): s2.push(s1.pop())
          return s2.pop()
  ```
- **Time Complexity:** Push $O(1)$, Pop Amortized $O(1)$ | **Space Complexity:** $O(N)$

---

## 10. Complexity Reference

| Operation / Structure | Enqueue | Dequeue | Peek | Auxiliary Space |
|---|:---:|:---:|:---:|:---:|
| Circular Array Queue | $O(1)$ | $O(1)$ | $O(1)$ | $O(N)$ |
| Linked List Queue | $O(1)$ | $O(1)$ | $O(1)$ | $O(N)$ |
| Queue using 2 Stacks | $O(1)$ | Amortized $O(1)$ | Amortized $O(1)$ | $O(N)$ |
| Stack using 2 Queues | $O(N)$ or $O(1)$ | $O(1)$ or $O(N)$ | $O(1)$ | $O(N)$ |
| Interleave Queue Halves | $O(N)$ | $O(N)$ | - | $O(N)$ |
| Reversing a Queue | $O(N)$ | $O(N)$ | - | $O(N)$ |

---

## 11. Common Mistakes

- **Using Naive Unshifted Array Queue:** Dequeuing without shifting elements leaves abandoned memory slots at the front. Always implement a **Circular Queue** with modulo arithmetic.
- **Mixing Up `poll()` and `remove()`:** Calling `remove()` on an empty Queue throws `NoSuchElementException`. Calling `poll()` returns `null` safely.
- **Confusing Deque Method Names:** Mistaking `peekFirst()` vs `peekLast()` or `offerFirst()` vs `offerLast()`.

---

## 12. Edge Cases

- **Empty Queue (`isEmpty() == true`).**
- **Single Element Queue.**
- **Queue at Full Capacity (Circular array overflow).**
- **Odd Length Queue during Interleaving.**

---

## 13. Interview Questions

### Beginner
1. What is the FIFO principle, and how does a Queue differ from a Stack?
2. Why is a Circular Array implementation superior to a standard linear array implementation for a Queue?

### Intermediate
1. Explain how to implement a Queue using two Stacks with amortized $O(1)$ time complexity.
2. How does `ArrayDeque` function as both a Stack and a Queue in Java?

### Advanced
1. Explain the algorithm and trace the data structure for finding the First Non-Repeating Character in a stream.
2. How do you implement a Stack using two Queues, and what are the trade-offs between Push-costly vs Pop-costly approaches?

---

## 14. Real-World Applications

- **CPU Task Scheduler:** OS ready queue managing process execution turns (Round Robin scheduling).
- **Print Spooler:** Print jobs executed in exact order of submission.
- **Graph Breadth-First Search (BFS):** Level-by-level tree and graph traversal.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`QueueArrayImplementation.java`](QueueArrayImplementation.java) | Circular Array implementation of Queue with modular arithmetic (`(rear + 1) % size`). |
| [`QueueLinkedListImplementation.java`](QueueLinkedListImplementation.java) | Singly Linked List implementation of Queue ($O(1)$ enqueue and dequeue). |
| [`QueueJCFDemo.java`](QueueJCFDemo.java) | Standard Java Collections Framework `Queue` interface using `LinkedList` and `ArrayDeque`. |
| [`DequeDemo.java`](DequeDemo.java) | Double-Ended Queue (`java.util.Deque`) methods (`addFirst`, `addLast`, `removeFirst`, `removeLast`). |
| [`QueueUsingTwoStacks.java`](QueueUsingTwoStacks.java) | Queue implementation backed by two Stacks achieving amortized $O(1)$ operations. |
| [`StackUsingTwoQueues.java`](StackUsingTwoQueues.java) | Stack LIFO data structure built using two Queues. |
| [`QueueUsingDeque.java`](QueueUsingDeque.java) | Queue wrapper implementation built on top of Java `Deque`. |
| [`StackUsingDeque.java`](StackUsingDeque.java) | Stack wrapper implementation built on top of Java `Deque`. |
| [`FirstNonRepeatingCharacter.java`](FirstNonRepeatingCharacter.java) | Finding first non-repeating character in a real-time stream using Queue + frequency map ($O(N)$ time). |
| [`InterleaveQueueHalves.java`](InterleaveQueueHalves.java) | Interleaving first half and second half of an even-length queue using auxiliary queue. |
| [`ReverseQueue.java`](ReverseQueue.java) | Reversing a Queue using a Stack helper data structure ($O(N)$ time). |

---

## 16. Related Topics

### Prerequisites
- Arrays, Singly Linked Lists, and Stacks.

### Related Topics
- Breadth-First Search (BFS) on Trees & Graphs.
- Sliding Window Maximum (Monotonic Deque).

### Next Topics
- Binary Tree Level-Order Traversal.
- Greedy Algorithms & Priority Queues.

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for Circular Queue array wrap or Queue Interleaving steps here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
