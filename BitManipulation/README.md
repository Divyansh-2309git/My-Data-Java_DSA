# Bit Manipulation

## 1. What Is This?

Bit Manipulation is the act of algorithmically manipulating individual bits (0s and 1s) inside binary representations of integers.

Because computers natively store and process data in binary at the hardware level, bitwise operations operate directly inside CPU registers without arithmetic overhead. They are extremely fast ($O(1)$ CPU cycles), memory-efficient, and essential for low-level systems programming, cryptography, feature flags, dynamic programming masks, and competitive programming optimizations.

---

## 2. Core Idea

An integer is represented in memory as a sequence of bits. In Java:
- `int` is a 32-bit signed two's complement integer.
- `long` is a 64-bit signed two's complement integer.

Bitwise operations process bits position by position independently:

```text
  Binary representation of 5:   0 0 1 0 1
  Binary representation of 6:   0 0 1 1 0
  ───────────────────────────────────────
  Bitwise AND (5 & 6):         0 0 1 0 0  --> 4
  Bitwise OR  (5 | 6):         0 0 1 1 1  --> 7
  Bitwise XOR (5 ^ 6):         0 0 0 1 1  --> 3
```

---

## 3. Important Terminology

| Operator / Term | Symbol | Truth Table / Definition |
|---|---|---|
| **Bitwise AND** | `&` | `1 & 1 = 1`, all other combinations `0`. |
| **Bitwise OR** | `\|` | `0 \| 0 = 0`, all other combinations `1`. |
| **Bitwise XOR** | `^` | `1 ^ 1 = 0`, `0 ^ 0 = 0`, `1 ^ 0 = 1` (Outputs 1 if bits differ!). |
| **Bitwise NOT** | `~` | Inverts all bits (`~0 = -1`, `~1 = -2` due to Two's Complement). |
| **Left Shift** | `<<` | Shifts bits left by $b$ positions ($a \ll b = a \times 2^b$). |
| **Right Shift** | `>>` | Shifts bits right by $b$ positions ($a \gg b = \lfloor a / 2^b \rfloor$). |
| **Bit Mask** | Mask | A binary number constructed to isolate, set, or clear specific target bit positions (e.g. `1 << i`). |

---

## 4. Visual / Mental Model

### Bit Masking (Isolating $i$-th Bit)

To inspect the $i$-th bit of number $N$, we shift `1` left by $i$ positions to create a Bit Mask:

```text
Number N = 13 (1101 in binary),  Target i = 2

Bit Mask (1 << 2):   0 1 0 0
Number N (13):       1 1 0 1
─────────────────────────────
N & (1 << 2):        0 1 0 0   --> Non-zero result! 2nd bit is SET (1).
```

### Bitwise XOR Magic Properties
1. $a \oplus a = 0$ (Self-cancellation)
2. $a \oplus 0 = a$ (Identity)
3. $a \oplus b \oplus a = b$ (Swapping without third variable!)

---

## 5. Operations / Techniques

### 1. Check Even or Odd
- `(n & 1) == 0` $\implies$ Even
- `(n & 1) != 0` $\implies$ Odd
- *Why it works:* LSB (Least Significant Bit at position 0) represents $2^0 = 1$. If set, number is odd.

### 2. Get $i$-th Bit
- `(n & (1 << i)) != 0` $\implies$ $i$-th bit is 1, otherwise 0.

### 3. Set $i$-th Bit (Force to 1)
- `n | (1 << i)`

### 4. Clear $i$-th Bit (Force to 0)
- `n & ~(1 << i)`

### 5. Clear Last $i$ Bits
- `n & (~0 << i)` where `~0` is all 1s (`11111111...`).

### 6. Fast Exponentiation ($a^b$ in $O(\log b)$ time)
- Inspect binary representation of exponent $b$. Multiply base $a$ when bit is set, square base $a$ at every step.

---

## 6. Worked Examples

### Worked Example 1: Check if Number is Power of 2

**Formula:** `(n > 0) && ((n & (n - 1)) == 0)`

**Input:** $N = 16$ (Binary `10000`)

- **Step 1:** Compute $N - 1 = 15$ (Binary `01111`).
- **Step 2:** Compute $16 \& 15$:
  ```text
    1 0 0 0 0   (16)
  & 0 1 1 1 1   (15)
  ───────────
    0 0 0 0 0   (0)
  ```
- **Result:** $16 \& 15 == 0 \implies \mathbf{true}$ (16 is a power of 2!).

### Worked Example 2: Fast Exponentiation ($3^5$)

Exponent $5$ in binary is `101` ($5 = 4 + 1$). Base $= 3$, Answer $= 1$.

- **Step 1 (Bit 0 = 1):** $5 \& 1 \neq 0$. Multiply $\text{ans} = 1 \times 3 = 3$. Square base: $3^2 = 9$. Shift exp right: $5 \gg 1 = 2$ (`10`).
- **Step 2 (Bit 1 = 0):** $2 \& 1 == 0$. Base squared: $9^2 = 81$. Shift exp right: $2 \gg 1 = 1$ (`1`).
- **Step 3 (Bit 2 = 1):** $1 \& 1 \neq 0$. Multiply $\text{ans} = 3 \times 81 = 243$. Shift exp right: $1 \gg 1 = 0$. Loop ends!
- **Result:** **`243`** computed in just 3 iterations instead of 5 multiplications!

---

## 7. Java Implementation Concepts

- **Signed Right Shift (`>>`) vs Unsigned Right Shift (`>>>`):**
  - `>>`: Arithmetic right shift (preserves the sign bit for negative numbers).
  - `>>>`: Logical right shift (fills left positions with zeros regardless of sign).
- **Two's Complement Representation:**
  - $-N = \sim N + 1$.
  - Inverting bits (`~N`) turns positive integer into negative integer $-N - 1$.

---

## 8. Problem-Solving Patterns

### Pattern 1: Bit Mask Construction
- **When to think of it:** Querying, setting, clearing, or toggling specific bits at index $i$.
- **Mental Approach:** Build mask `1 << i` or `~(1 << i)` and combine using `&`, `|`, or `^`.

### Pattern 2: Brian Kernighan Bit Clearing (`n & (n - 1)`)
- **When to think of it:** Counting total set bits (1s) in an integer, checking power of 2.
- **Mental Approach:** $n \& (n - 1)$ clears the rightmost set bit of $n$ in $O(1)$ time.

---

## 9. Algorithms

### Fast Exponentiation Algorithm
- **Problem solved:** Compute $a^b \pmod m$ in $O(\log b)$ logarithmic time.
- **Pseudocode:**
  ```text
  function fastPow(a, b):
      ans = 1
      while b > 0:
          if (b & 1) != 0: // LSB is set
              ans = ans * a
          a = a * a   // Square base
          b = b >> 1  // Right shift exponent
      return ans
  ```
- **Time Complexity:** $O(\log b)$ | **Space Complexity:** $O(1)$

---

## 10. Complexity Reference

| Operation / Trick | Time Complexity | Space Complexity |
|---|---|---|
| Bitwise AND, OR, XOR, NOT | $O(1)$ | $O(1)$ |
| Bit Shifts (`<<`, `>>`, `>>>`) | $O(1)$ | $O(1)$ |
| Get / Set / Clear $i$-th Bit | $O(1)$ | $O(1)$ |
| Check Even / Odd | $O(1)$ | $O(1)$ |
| Check Power of 2 | $O(1)$ | $O(1)$ |
| Count Set Bits (Kernighan) | $O(\text{set bits}) \le 32$ | $O(1)$ |
| Fast Exponentiation ($a^b$) | $O(\log b)$ | $O(1)$ |

---

## 11. Common Mistakes

- **Operator Precedence Confusion:** Bitwise operators (`&`, `|`, `^`) have LOWER precedence than comparison operators (`==`, `!=`, `<`, `>`). Always use parentheses!
  - Incorrect: `if (n & 1 == 0)` evaluates as `if (n & (1 == 0))`!
  - Correct: `if ((n & 1) == 0)`.
- **Shift Overflows:** Shifting an `int` by $\ge 32$ positions (e.g. `1 << 35`) results in `1 << (35 % 32) = 1 << 3`. Use `1L << 35` for 64-bit shifting!

---

## 12. Edge Cases

- **Negative Integers:** Sign bit (31st bit) is `1`. Shift operations must distinguish `>>` and `>>>`.
- **$N = 0$:** Zero has zero set bits; not a power of 2.
- **Integer Overflow:** Bit shifts exceeding $2^{31}-1$.
- **`Integer.MIN_VALUE` ($-2^{31}$):** Overflow when taking absolute value `-Integer.MIN_VALUE`.

---

## 13. Interview Questions

### Beginner
1. How does `n & 1` determine whether integer $n$ is even or odd?
2. Explain the difference between `>>` (signed shift) and `>>>` (unsigned shift) in Java.

### Intermediate
1. Explain why `(n & (n - 1)) == 0` correctly identifies powers of two.
2. How does Fast Exponentiation achieve $O(\log b)$ time complexity using binary bits?

### Advanced
1. Implement single number lookup in an array where every element appears twice except one (using XOR cancellation).
2. Explain Two's Complement representation of negative integers in 32-bit systems.

---

## 14. Real-World Applications

- **Feature Flags & Permissions:** Storing 32 boolean permissions in a single 32-bit `int` bitmask (e.g. Read=1, Write=2, Execute=4).
- **Graphics & Color Buffers:** Packing ARGB color channels (8 bits Alpha, 8 bits Red, 8 bits Green, 8 bits Blue) into a single 32-bit integer.
- **Network Subnetting:** IP netmasks and subnet routing tables.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`BitwiseOperatorsDemo.java`](BitwiseOperatorsDemo.java) | Demonstration of AND (`&`), OR (`\|`), XOR (`^`), NOT (`~`), Left Shift (`<<`), and Right Shift (`>>`). |
| [`CheckEvenOddBitwise.java`](CheckEvenOddBitwise.java) | Fast $O(1)$ even/odd check testing the least significant bit (`n & 1`). |
| [`GetIthBit.java`](GetIthBit.java) | Retrieving the binary bit value (0 or 1) at $i$-th position using bit mask `1 << i`. |
| [`SetIthBit.java`](SetIthBit.java) | Forcing the $i$-th bit of a number to 1 using bitwise OR (`n \| (1 << i)`). |

---

## 16. Related Topics

### Prerequisites
- Java Basics & Binary Number Systems.

### Related Topics
- Computer Organization & Assembly.
- Math & Modular Arithmetic.

### Next Topics
- Dynamic Programming with Bitmasking.
- State Compression in Graphs.

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add visual notes for Bit Masking or Fast Exponentiation trace here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
