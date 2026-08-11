# Java Basics & Fundamental Programming

## 1. What Is This?

Java Basics forms the essential bedrock of computer programming and problem-solving in Data Structures and Algorithms (DSA). Before building advanced data structures like trees or graphs, every programmer must understand how data is stored, manipulated, and controlled through program flow.

This module introduces core Java syntax, variables, operators, decision-making constructs (`if-else`, `switch`), iteration loops (`for`, `while`), modular function design, call stack mechanics, and foundational mathematical algorithms.

---

## 2. Core Idea

Programs execute sequentially line-by-line, branching based on logical conditions, and looping over repeatable tasks until a stopping condition is met. Functions allow breaking complex code into reusable, modular blocks.

```text
       ┌────────────────────────┐
       │   Input / Variables    │
       └───────────┬────────────┘
                   │
                   ▼
       ┌────────────────────────┐
       │   Control Flow / Loops │
       └───────────┬────────────┘
                   │
                   ▼
       ┌────────────────────────┐
       │ Functions / Algorithms │
       └───────────┬────────────┘
                   │
                   ▼
       ┌────────────────────────┐
       │    Output / Result     │
       └────────────────────────┘
```

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Variable** | A named memory location that holds a value of a specific data type. |
| **Primitive Type** | Core building block data types built into Java (`int`, `double`, `boolean`, `char`, etc.). |
| **Operator** | A symbol that tells the compiler to perform specific mathematical or logical manipulations. |
| **Control Flow** | The order in which individual statements, instructions, or function calls are executed. |
| **Call Stack** | A stack data structure that stores information about active subroutines/functions in execution. |
| **Pass-by-Value** | Java's mechanism of passing arguments to methods by copying their primitive values or reference copies. |
| **Method Overloading** | Defining multiple methods in the same class with the same name but different parameter lists. |

---

## 4. Visual / Mental Model

### Call Stack Mental Model (Method Execution)

When `main()` calls `isPrime(13)`, a new frame is pushed onto the stack. When `isPrime()` returns `true`, its frame is popped.

```text
 ┌──────────────────────────┐
 │ isPrime(n = 13) Frame    │  <-- Active Execution
 ├──────────────────────────┤
 │ main() Frame             │  <-- Suspended waiting for result
 └──────────────────────────┘
```

### Number Reversal Mental Model

Extracting digits using modulo `% 10` and stripping digits using integer division `/ 10`:

```text
Number: 1234
  1234 % 10 = 4   --->  rev = 0 * 10 + 4 = 4     --->  number = 1234 / 10 = 123
   123 % 10 = 3   --->  rev = 4 * 10 + 3 = 43    --->  number = 123 / 10 = 12
    12 % 10 = 2   --->  rev = 43 * 10 + 2 = 432  --->  number = 12 / 10 = 1
     1 % 10 = 1   --->  rev = 432 * 10 + 1 = 4321-->  number = 1 / 10 = 0 (Stop)
```

---

## 5. Operations / Techniques

### 1. Decision Making (`if-else`, `switch`)
Evaluates boolean expressions to divert execution down different code paths.

### 2. Iteration (`for`, `while`, `do-while`)
Repeats a block of code as long as a specified condition is `true`. `for` loops are preferred when iteration count is known; `while` loops when termination depends on dynamic conditions.

### 3. Digit Extraction Loop
Repeatedly uses `num % 10` to get the last digit and `num /= 10` to drop the last digit.

### 4. Optimized Prime Testing
Checks divisors up to $\sqrt{n}$ instead of $n$. If $n$ has a factor greater than $\sqrt{n}$, it must also have a corresponding factor smaller than $\sqrt{n}$.

---

## 6. Worked Examples

### Worked Example 1: Reversing an Integer

**Input:** `num = 456`

- **Step 1:** Initialize `rev = 0`.
- **Step 2:** `lastDigit = 456 % 10 = 6`. `rev = 0 * 10 + 6 = 6`. `num = 456 / 10 = 45`.
- **Step 3:** `lastDigit = 45 % 10 = 5`. `rev = 6 * 10 + 5 = 65`. `num = 45 / 10 = 4`.
- **Step 4:** `lastDigit = 4 % 10 = 4`. `rev = 65 * 10 + 4 = 654`. `num = 4 / 10 = 0`.
- **Result:** `654`

### Worked Example 2: Checking for Prime Number ($\sqrt{n}$ approach)

**Input:** `n = 37`

- **Step 1:** Edge cases check: `n < 2` is `false`. `n == 2` is `false`.
- **Step 2:** Compute limit $\sqrt{37} \approx 6.08$. Loop $i$ from `2` to `6`.
- **Step 3:** $i=2$: $37 \% 2 \neq 0$.
- **Step 4:** $i=3$: $37 \% 3 \neq 0$.
- **Step 5:** $i=4$: $37 \% 4 \neq 0$.
- **Step 6:** $i=5$: $37 \% 5 \neq 0$.
- **Step 7:** $i=6$: $37 \% 6 \neq 0$.
- **Result:** Loop completes with no divisor found $\implies$ `true` (37 is Prime).

---

## 7. Java Implementation Concepts

- **Scanner for Input:** `Scanner sc = new Scanner(System.in);` reads user input from standard console.
- **Primitive Promotion:** `int / int` performs integer truncation. Use `(double) a / b` for floating point division.
- **Math Utility Class:** `Math.sqrt(n)`, `Math.pow(base, exp)`, `Math.max(a, b)`, `Math.PI`.
- **Method Overloading Rules:** Methods can share a name if parameters differ in count, types, or order. Return type alone is insufficient to overload.

---

## 8. Problem-Solving Patterns

### Pattern 1: Modulo-Division Loop
- **When to think of it:** Processing digits of a number, reversing a number, checking palindromes, converting bases.
- **Mental Approach:** Extract last digit (`% 10`), update accumulator, shrink number (`/ 10`).

### Pattern 2: Square Root Boundary Loop
- **When to think of it:** Primality testing, finding all factors of a number.
- **Mental Approach:** Loop $i$ up to $i \times i \le n$. Process both $i$ and $n / i$.

---

## 9. Algorithms

### Optimized Primality Test Algorithm
- **Problem solved:** Determine if integer $n$ is prime.
- **Core Idea:** No composite number $n$ can have all prime factors greater than $\sqrt{n}$.
- **Pseudocode:**
  ```text
  function isPrime(n):
      if n <= 1 return false
      if n == 2 return true
      for i = 2 to sqrt(n):
          if n % i == 0 return false
      return true
  ```
- **Time Complexity:** $O(\sqrt{n})$
- **Space Complexity:** $O(1)$
- **Common Mistakes:** Checking up to $n-1$ instead of $\sqrt{n}$ ($O(n)$ time wasting).

---

## 10. Complexity Reference

| Operation / Algorithm | Time Complexity | Space Complexity |
|---|---|---|
| Basic Arithmetic / Bitwise | $O(1)$ | $O(1)$ |
| Conditional Statement | $O(1)$ | $O(1)$ |
| Counting Loop ($1$ to $N$) | $O(N)$ | $O(1)$ |
| Digit Extraction / Reversal | $O(\log_{10} N)$ | $O(1)$ |
| Naive Prime Check | $O(N)$ | $O(1)$ |
| Optimized Prime Check ($\sqrt{N}$) | $O(\sqrt{N})$ | $O(1)$ |
| Decimal-to-Binary Conversion | $O(\log_2 N)$ | $O(1)$ |

---

## 11. Common Mistakes

- **Integer Overflow:** Multiplying large integers leading to negative overflow. Use `long` if values exceed $2^{31}-1$.
- **Off-by-One in Loops:** Writing `i < n` instead of `i <= n` or vice versa.
- **Floating Point Comparison:** Comparing floats directly with `==` instead of `Math.abs(a - b) < 1e-9`.
- **Infinite Loop:** Forgetting to update loop counter inside `while` loop body.
- **Dangling Else:** Misunderstanding nested `if-else` blocks due to missing curly braces `{}`.

---

## 12. Edge Cases

- **Negative Numbers:** Handling negative integers during reversal or digit extraction.
- **Zero Input:** `num = 0` in digit counting or loop conditions.
- **Boundary Prime Values:** `n = 0`, `n = 1` (neither prime nor composite), `n = 2` (only even prime).
- **Large Prime Numbers:** Values near `Integer.MAX_VALUE`.

---

## 13. Interview Questions

### Beginner
1. What is the difference between `i++` (post-increment) and `++i` (pre-increment)?
2. How does Java handle `int` division vs `double` division?

### Intermediate
1. Explain how `isPrime(n)` can be optimized from $O(n)$ to $O(\sqrt{n})$.
2. What is method overloading, and how does the compiler select the correct method signature?

### Advanced
1. How does the Java Call Stack manage function execution and primitive parameter passing?
2. Explain potential precision issues when working with `float` and `double` in financial calculations.

---

## 14. Real-World Applications

- **Financial Calculators:** Calculating interest rates, monthly payments, currency conversions.
- **Validation Engine:** Checking account numbers using digit extraction algorithms (e.g. Luhn algorithm).
- **Cryptography Basics:** Primality testing for RSA key generation and modulo arithmetic in ciphers.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`BinaryDecimalConversion.java`](BinaryDecimalConversion.java) | Decimal-to-binary and binary-to-decimal base conversion using bit math and positional powers. |
| [`Calculator.java`](Calculator.java) | Interactive switch-case based menu calculator performing arithmetic operations. |
| [`CircleAreaCalculation.java`](CircleAreaCalculation.java) | Formula calculation using `Math.PI` and radius multiplication. |
| [`ConditionalStatements.java`](ConditionalStatements.java) | Conditional branching (`if`, `else if`, `else`) and ternary operators. |
| [`ForLoopsDemo.java`](ForLoopsDemo.java) | `for` loop iteration, syntax, counter manipulation, and range printing. |
| [`FunctionOverloading.java`](FunctionOverloading.java) | Method overloading by changing parameter count and types. |
| [`FunctionsDemo.java`](FunctionsDemo.java) | Function definition, parameters, return values, call stack, and pass-by-value. |
| [`IsPrimeCheck.java`](IsPrimeCheck.java) | Naive vs optimized primality testing using $\sqrt{n}$ boundary limit. |
| [`OperatorsDemo.java`](OperatorsDemo.java) | Demonstration of arithmetic, relational, logical, assignment, and unary operators. |
| [`PatternPrinting.java`](PatternPrinting.java) | Nested loops for printing star patterns, half-pyramids, and character matrices. |
| [`PrimeFunctionDemo.java`](PrimeFunctionDemo.java) | Modular prime helper function and generating primes in a range. |
| [`ReverseNumber.java`](ReverseNumber.java) | Number reversal using modulo `% 10` digit extraction and integer division `/ 10`. |
| [`WhileLoopsDemo.java`](WhileLoopsDemo.java) | `while` and `do-while` loops for condition-driven iteration. |

---

## 16. Related Topics

### Prerequisites
- Basic computer literacy and terminal execution commands (`javac`, `java`).

### Related Topics
- Bit Manipulation (binary representation and bitwise operators).
- Math & Number Theory algorithms.

### Next Topics
- Arrays & 1D Data Storage.
- Control structures in Object-Oriented Programming (OOP).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add handwritten notes or flowcharts for Java execution, call stack diagrams, or loop state transitions here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
