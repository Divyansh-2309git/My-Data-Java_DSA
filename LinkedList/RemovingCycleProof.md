# Mathematical Proof of Linked List Cycle Detection & Removal

![Mathematical Proof of Linked List Cycle Removal](image.png)

## Mathematical Proof Derivation

Let:
- $x$ = Distance from `head` to the cycle entry node.
- $y$ = Distance from cycle entry node to the meeting point of `slow` and `fast` pointers.
- $C$ = Length / circumference of the cycle.

```text
head
 ───► [ Node ] ───► ... ───► [ Cycle Entry ] ───► ... ───► [ Meeting Point ]
         ◄─────── x ────────►                 ◄───── y ───►
```

### 1. Meeting Condition
The `fast` pointer moves at 2x the speed of the `slow` pointer:

$$\text{Distance(fast)} = 2 \times \text{Distance(slow)}$$

When they meet inside the cycle:
- $\text{Distance(slow)} = x + y$
- $\text{Distance(fast)} = x + y + k \cdot C$ (where $k \ge 1$ is the number of complete loop rotations made by `fast`).

Substituting into the speed equation:

$$x + y + k \cdot C = 2(x + y)$$

$$x + y + k \cdot C = 2x + 2y$$

$$k \cdot C = x + y$$

$$x = k \cdot C - y$$

$$x = (k - 1)C + (C - y)$$

---

### 2. Proof of Cycle Entry & Removal
The term $(C - y)$ represents the remaining distance from the meeting point to complete the cycle and return to the cycle entry node!

If we reset `slow = head` and leave `fast` at the meeting point:
- Moving both pointers **one step at a time**:
  - `slow` covers distance $x$ from `head` to reach the cycle entry node.
  - `fast` covers distance $x = (k - 1)C + (C - y)$ from the meeting point. Since $(k-1)C$ is full loops and $(C - y)$ finishes the loop, `fast` also arrives at the **exact same cycle entry node**!

### 3. Unlinking the Cycle
Once `slow.next == fast.next`, `fast` is pointing to the last node in the cycle (predecessor to entry node). Setting `fast.next = null` breaks the loop cleanly!