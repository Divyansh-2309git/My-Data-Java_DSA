# Singly Linked List Data Structure

## 1. What Is This?

A Linked List is a linear data structure consisting of discrete nodes connected by memory pointers/references rather than stored in contiguous array memory.

Unlike arrays where elements are placed side-by-side in memory, linked list nodes can be scattered across the heap. Each node contains two fields:
1. **Data:** The value stored in the node.
2. **Next:** A reference/pointer pointing to the memory address of the next node in the list.

Linked Lists excel at dynamic insertions and deletions at known positions in $O(1)$ constant time without requiring memory reallocation or element shifting.

---

## 2. Core Idea

The list is accessed via the `head` reference (first node). The final node's `next` reference points to `null`, signaling the end of the list.

```text
  head                                                   tail
   │                                                      │
   ▼                                                      ▼
 ┌───────────┐     ┌───────────┐     ┌───────────┐     ┌───────────┐
 │ Data |Next│ ──► │ Data |Next│ ──► │ Data |Next│ ──► │ Data |null│
 └───────────┘     └───────────┘     └───────────┘     └───────────┘
```

To access any index $i$, we must start at `head` and traverse node-by-node down the reference chain in $O(N)$ time.

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Node** | A custom object holding data payload and pointer reference (`next`). |
| **Head** | Reference variable pointing to the first node in the list. |
| **Tail** | Reference variable pointing to the last node in the list (`tail.next == null`). |
| **Null Pointer** | Signal that a reference points to no object (`next == null`). |
| **Slow & Fast Pointers** | Two-pointer technique (Floyd's Cycle / Hare & Tortoise) moving at 1 step and 2 steps per iteration. |
| **In-Place Reversal** | Reversing link pointers (`curr.next = prev`) without creating new node objects. |

---

## 4. Visual / Mental Model

### Singly Linked List Reversal Trace

To reverse a linked list `10 -> 20 -> 30 -> null`:

```text
Initial:   prev = null,  curr = [10] ──► [20] ──► [30] ──► null

Step 1:    next = curr.next ([20])
           curr.next = prev (null)
           prev = curr ([10])
           curr = next ([20])
           State:  null ◄── [10]   [20] ──► [30] ──► null
                           prev    curr

Step 2:    State:  null ◄── [10] ◄── [20]   [30] ──► null
                                    prev    curr

Step 3:    State:  null ◄── [10] ◄── [20] ◄── [30]   null
                                             prev    curr (null!)
Update head = prev ([30])!
```

---

## 5. Operations / Techniques

| Operation | Description | Time Complexity | Space Complexity |
|---|---|---|---|
| **Add First** | Create node, `newNode.next = head`, `head = newNode` | $O(1)$ | $O(1)$ |
| **Add Last** | `tail.next = newNode`, `tail = newNode` | $O(1)$ with tail | $O(1)$ |
| **Remove First**| `head = head.next` | $O(1)$ | $O(1)$ |
| **Remove Last** | Traverse to $N-2$ node, set `prev.next = null`, `tail = prev` | $O(N)$ | $O(1)$ |
| **Search (Iter)**| Loop `temp = temp.next` matching `temp.data == key` | $O(N)$ | $O(1)$ |
| **Search (Rec)** | Recursive scan forwarding index marker | $O(N)$ | $O(N)$ stack |
| **Reverse List** | Pointer manipulation swapping `curr.next` to `prev` | $O(N)$ | $O(1)$ |
| **Find Middle** | Slow/Fast pointer (slow moves 1, fast moves 2) | $O(N)$ | $O(1)$ |
| **Cycle Check** | Floyd's Cycle Detection (meeting of slow and fast) | $O(N)$ | $O(1)$ |

---

## 6. Worked Examples

### Worked Example: Floyd's Cycle Detection & Removal

**List Structure:** $1 \to 2 \to 3 \to 4 \to 5 \to 3$ (Cycle created from node 5 back to node 3).

- **Step 1 (Cycle Detection):**
  - Place `slow = head` (1), `fast = head` (1).
  - Iteration 1: `slow` = 2, `fast` = 3.
  - Iteration 2: `slow` = 3, `fast` = 5.
  - Iteration 3: `slow` = 4, `fast` = 4 $\implies$ **`slow == fast` at node 4! Cycle detected!**
- **Step 2 (Locate Entry & Break Cycle):**
  - Reset `slow = head` (1). Keep `fast` at node 4.
  - Move both 1 step at a time:
    - Step 1: `slow` = 2, `fast` = 5.
    - Step 2: `slow` = 3, `fast` = 3 $\implies$ **Cycle Entry found at node 3!**
  - Track previous node during fast pointer movement (node 5). Set `prev.next = null`.
- **Result:** Cycle removed! List becomes linear $1 \to 2 \to 3 \to 4 \to 5 \to \text{null}$.

---

## 7. Java Implementation Concepts

- **Static Inner Node Class:**
  ```java
  public static class Node {
      int data;
      Node next;
      public Node(int data) {
          this.data = data;
          this.next = null;
      }
  }
  ```
- **Dereferencing Safeguard:** Always check `head != null` and `curr.next != null` before writing `curr.next.data` to prevent `NullPointerException`.
- **Garbage Collection of Nodes:** In Java, setting `head = head.next` or unlinking a node (`prev.next = curr.next`) drops all references to `curr`. The JVM Garbage Collector automatically reclaims its memory!

---

## 8. Problem-Solving Patterns

### Pattern 1: Slow and Fast Pointers (Tortoise & Hare)
- **When to think of it:** Finding middle node, cycle detection, finding $K$-th node from end, checking palindrome linked list.
- **Mental Approach:** Move `slow = slow.next` (1 step) and `fast = fast.next.next` (2 steps).

### Pattern 2: Pointer Rewiring (Reverse / Relink)
- **When to think of it:** Reversing list, reordering list, merging two lists.
- **Mental Approach:** Maintain three pointer markers: `prev`, `curr`, and `next`. Store `next = curr.next` BEFORE mutating `curr.next`!

---

## 9. Algorithms

### 1. Reverse Singly Linked List
- **Pseudocode:**
  ```text
  function reverseList(head):
      prev = null, curr = head
      while curr != null:
          next = curr.next
          curr.next = prev
          prev = curr
          curr = next
      return prev // New head!
  ```
- **Time Complexity:** $O(N)$ | **Space Complexity:** $O(1)$

### 2. Remove $N$-th Node From End
- **Pseudocode:**
  ```text
  function removeNthFromEnd(head, n):
      size = countNodes(head)
      if n == size: return head.next // Remove head
      prevIdx = size - n
      prev = head
      for i = 1 to prevIdx - 1: prev = prev.next
      prev.next = prev.next.next
      return head
  ```
- **Time Complexity:** $O(N)$ | **Space Complexity:** $O(1)$

---

## 10. Complexity Reference

| Operation | Array | Singly Linked List |
|---|:---:|:---:|
| Access by Index (`get(i)`) | $O(1)$ | $O(N)$ |
| Search Value | $O(N)$ | $O(N)$ |
| Insert at Head / Beginning | $O(N)$ | $O(1)$ |
| Insert at Tail (with tail ref) | Amortized $O(1)$ | $O(1)$ |
| Delete at Head | $O(N)$ | $O(1)$ |
| Delete at Tail | $O(1)$ | $O(N)$ (requires finding $N-2$) |

---

## 11. Common Mistakes

- **Losing List Reference (Pointer Disconnection):** Changing `curr.next = prev` before capturing `next = curr.next`, severing access to the rest of the list.
- **NullPointerExceptions on Fast Pointer:** Writing `fast.next.next` without checking both `fast != null` and `fast.next != null`.
- **Stale Head/Tail Updates:** Forgetting to update `head` or `tail` pointers when removing the single remaining node in a 1-element list.

---

## 12. Edge Cases

- **Empty List (`head == null`).**
- **Single Node List (`head.next == null`).**
- **Two Node List.**
- **Removing Head Node ($N$-th from end where $N = \text{size}$).**
- **List with Cycle at Head Node.**

---

## 13. Interview Questions

### Beginner
1. Compare Arrays and Linked Lists regarding memory layout, index access, and insertion speed.
2. How do you reverse a Singly Linked List in $O(N)$ time and $O(1)$ space?

### Intermediate
1. Explain Floyd's Cycle Finding Algorithm (Slow and Fast Pointers) for loop detection.
2. How do you find the middle node of a Linked List in a single pass?

### Advanced
1. Derive the complete mathematical proof for Floyd's Cycle Removal algorithm (refer to [RemovingCycleProof.md](RemovingCycleProof.md)).
2. How do you check if a Linked List is a Palindrome in $O(N)$ time and $O(1)$ auxiliary space? (Find middle $\to$ Reverse second half $\to$ Compare halves).

---

## 14. Real-World Applications

- **Operating System Memory Management:** Free memory block allocation tables (Free Lists).
- **Undo / Redo Buffers:** Text editors holding editing history nodes.
- **Browser History Back/Forward:** Doubly linked lists tracking web page navigation history.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`SinglyLinkedListDemo.java`](SinglyLinkedListDemo.java) | Custom Singly Linked List class featuring `addFirst`, `addLast`, `removeFirst`, `removeLast`, `searchIterative`, `searchRecursive`, `reverse`, `removeNthFromEnd`, `checkPalindrome`, `hasCycle`, and `removeCycle`. |
| [`RemovingCycleProof.md`](RemovingCycleProof.md) | Dedicated mathematical proof markdown document explaining Floyd's cycle entry equation ($x = kC - y$) and cycle unlinking. |

---

## 16. Related Topics

### Prerequisites
- Object-Oriented Programming (Classes & Heap References).

### Related Topics
- Stacks & Queues (Linked List implementations).
- Doubly & Circular Linked Lists.

### Next Topics
- Stacks (LIFO Data Structure).
- Queues (FIFO Data Structure).

---

# 🖼️ 17. Visual Notes / Personal Images

## Personal Visual Notes & Proof

The repository contains personal visual notes and mathematical proofs for Linked List algorithms:

![Linked List Personal Visual Notes](image.png)

### What the image represents
- **Cycle Detection & Removal Geometry:** Visual diagram showing distances $x$ (head to loop start), $y$ (loop start to meeting point), and $C$ (cycle perimeter).
- **Pointer Rewiring Mechanics:** Pointer transitions during node additions, deletions, and list reversal.
- **Detailed Proof:** For full mathematical equations and step-by-step derivation, see [RemovingCycleProof.md](RemovingCycleProof.md).
