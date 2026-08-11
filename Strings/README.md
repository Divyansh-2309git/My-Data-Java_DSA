# Strings & StringBuilder

## 1. What Is This?

A String is a sequence of characters. In Java, `String` is an object class (`java.lang.String`) rather than a primitive data type.

Java strings have a crucial property: **IMMUTABILITY**. Once a `String` object is created in memory, its value cannot be modified. Any operation that appears to modify a string (such as concatenation or substring replacement) actually creates an entirely new `String` object in the heap.

To address performance overhead caused by immutability during heavy string building, Java provides `StringBuilder`—a mutable, resizable character array.

---

## 2. Core Idea

### String Constant Pool & Heap Allocation

Java maintains a special memory region called the **String Constant Pool (SCP)** inside the Heap:

- When creating strings with literals (`String s1 = "hello";`), Java checks if `"hello"` exists in the SCP. If present, it reuses the reference.
- When creating strings with `new` (`String s2 = new String("hello");`), Java creates a brand-new object on the Heap outside the pool.

```text
HEAP MEMORY
 ┌───────────────────────────────────────────┐
 │  String Constant Pool (SCP)               │
 │  ┌───────────────┐                        │
 │  │    "hello"    │  <── s1, s3            │
 │  └───────────────┘                        │
 └───────────────────────────────────────────┘
   ┌───────────────┐
   │    "hello"    │   <── s2 (new Object)
   └───────────────┘
```

Because of this memory optimization:
- `s1 == s3` evaluates to `true` (same memory address in pool).
- `s1 == s2` evaluates to `false` (different heap objects!).
- `s1.equals(s2)` evaluates to `true` (identical character content!).

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Immutability** | Unchangeable state; once created, string object internal character bytes cannot be altered. |
| **String Constant Pool (SCP)** | A dedicated pool in heap memory that stores unique string literals for memory optimization. |
| **Reference Equality (`==`)** | Checks whether two variables point to the exact same memory address. |
| **Content Equality (`.equals()`)**| Checks whether two strings contain the identical sequence of characters. |
| **Lexicographical Order** | Dictionary order based on Unicode/ASCII integer values of characters. |
| **`StringBuilder`** | A mutable sequence of characters allowing $O(1)$ amortized character appends. |

---

## 4. Visual / Mental Model

### Concatenation Overhead vs `StringBuilder`

Performing string concatenation in a loop:

```java
String s = "";
for (char ch = 'a'; ch <= 'z'; ch++) {
    s = s + ch; // CREATES A NEW OBJECT IN EVERY ITERATION!
}
```

- Iteration 1: `"a"` (1 char)
- Iteration 2: `"ab"` (2 chars)
- ...
- Iteration $N$: Total characters copied $= 1 + 2 + 3 + \dots + N = O(N^2)$ time!

Using `StringBuilder`:

```text
[ a ][ b ][ c ][ d ] ... [ z ]  --> Modifies single underlying array in O(N) time!
```

---

## 5. Operations / Techniques

| Operation | Java API | Time Complexity | Space Complexity |
|---|---|---|---|
| **Length** | `str.length()` | $O(1)$ | $O(1)$ |
| **Character Access** | `str.charAt(index)` | $O(1)$ | $O(1)$ |
| **Substring Extraction** | `str.substring(start, end)` | $O(\text{length})$ | $O(\text{length})$ |
| **Content Comparison** | `str1.equals(str2)` | $O(N)$ | $O(1)$ |
| **Lexicographical Compare**| `str1.compareTo(str2)` | $O(\min(N, M))$ | $O(1)$ |
| **Concatenation (`+`)** | `str1 + str2` | $O(N + M)$ | $O(N + M)$ |
| **`StringBuilder` Append**| `sb.append(ch)` | Amortized $O(1)$ | $O(1)$ |

---

## 6. Worked Examples

### Worked Example 1: Palindrome Check (Two Pointers)

**Input String:** `"racecar"`

- **Step 1:** Initialize `left = 0`, `right = 6`.
- **Step 2:** `str.charAt(0) == 'r'`, `str.charAt(6) == 'r'` $\implies$ Match! Increment `left = 1`, decrement `right = 5`.
- **Step 3:** `str.charAt(1) == 'a'`, `str.charAt(5) == 'a'` $\implies$ Match! Increment `left = 2`, decrement `right = 4`.
- **Step 4:** `str.charAt(2) == 'c'`, `str.charAt(4) == 'c'` $\implies$ Match! Increment `left = 3`, decrement `right = 3`.
- **Step 5:** `left == right` (pointers meet at center `'e'`). Loop terminates.
- **Result:** **`true`** (String is a valid palindrome).

### Worked Example 2: String Compression (Run-Length Encoding)

**Input String:** `"aaabbccc"`

- **Step 1:** Loop $i$ from `0` to `n-1`. Initialize `count = 1`.
- **Step 2:** $i=0$: `'a'`. Next chars at $i=1, 2$ are also `'a'`. `count` becomes `3`. Append `'a'` and `3` to `StringBuilder` $\implies$ `"a3"`.
- **Step 3:** Move $i$ to index `3`: `'b'`. Next char at $i=4$ is `'b'`. `count` becomes `2`. Append `'b'` and `2` $\implies$ `"a3b2"`.
- **Step 4:** Move $i$ to index `5`: `'c'`. Next chars at $i=6, 7$ are `'c'`. `count` becomes `3`. Append `'c'` and `3` $\implies$ `"a3b2c3"`.
- **Result:** **`"a3b2c3"`**

---

## 7. Java Implementation Concepts

- **String Literals vs Object Instantiation:**
  ```java
  String a = "java";                  // Uses String Pool
  String b = new String("java");       // Creates heap object
  System.out.println(a == b);         // false
  System.out.println(a.equals(b));    // true
  ```
- **`StringBuilder` Usage:**
  ```java
  StringBuilder sb = new StringBuilder("");
  for (char ch = 'a'; ch <= 'z'; ch++) {
      sb.append(ch);
  }
  String result = sb.toString();
  ```
- **Lexicographical Comparison (`compareTo`):**
  - Returns `0` if strings are equal.
  - Returns `< 0` if `str1` precedes `str2` in dictionary order.
  - Returns `> 0` if `str1` follows `str2` in dictionary order.

---

## 8. Problem-Solving Patterns

### Pattern 1: Two-Pointer Symmetrical Scan
- **When to think of it:** Palindrome verification, string reversal.
- **Mental Approach:** Move `left` from start and `right` from end towards center, matching `str.charAt(left) == str.charAt(right)`.

### Pattern 2: `StringBuilder` Accumulator
- **When to think of it:** String modification, word capitalization, string compression, removing duplicates.
- **Mental Approach:** Process characters lineally, append to `StringBuilder`, convert `sb.toString()` at end.

---

## 9. Algorithms

### String Compression Algorithm
- **Problem solved:** Compress consecutive repeating characters into character + frequency count.
- **Pseudocode:**
  ```text
  function compress(str):
      sb = new StringBuilder()
      for i = 0 to str.length() - 1:
          count = 1
          while i < str.length() - 1 and str.charAt(i) == str.charAt(i+1):
              count++
              i++
          sb.append(str.charAt(i))
          if count > 1 sb.append(count)
      return sb.toString()
  ```
- **Time Complexity:** $O(N)$ | **Space Complexity:** $O(N)$ for result

---

## 10. Complexity Reference

| Operation / Algorithm | Time Complexity | Space Complexity |
|---|---|---|
| String Concatenation in Loop (`+`) | $O(N^2)$ | $O(N^2)$ |
| `StringBuilder` Concatenation Loop | $O(N)$ | $O(N)$ |
| Palindrome Check | $O(N)$ | $O(1)$ |
| Substring Extraction | $O(K)$ ($K = \text{length}$) | $O(K)$ |
| String Compression | $O(N)$ | $O(N)$ |
| Capitalize Words | $O(N)$ | $O(N)$ |
| Lexicographical Comparison | $O(N)$ | $O(1)$ |

---

## 11. Common Mistakes

- **Comparing Strings with `==`:** Writing `if (str1 == str2)` tests memory references instead of content equality. ALWAYS use `str1.equals(str2)`.
- **String Concatenation in Loops:** Using `+` inside loops causes severe $O(N^2)$ performance degradation due to object reallocation. Always use `StringBuilder`.
- **`StringIndexOutOfBoundsException`:** Calling `str.charAt(n)` or `str.substring(0, n+1)` where index exceeds `str.length()`.

---

## 12. Edge Cases

- **Empty String (`""`):** `length() == 0`.
- **Single Character String (`"a"`):** Always a palindrome.
- **String with Spaces & Punctuation:** Handling uppercase/lowercase conversion and space separation.
- **All Identical Characters (`"aaaaa"`).**

---

## 13. Interview Questions

### Beginner
1. What does it mean that Java Strings are immutable?
2. Explain the difference between `==` and `.equals()` when comparing Strings.

### Intermediate
1. Why is `StringBuilder` faster than String concatenation (`+`) inside a loop?
2. How does the String Constant Pool (SCP) optimize memory in the Java Heap?

### Advanced
1. Implement String Compression in-place without creating auxiliary string copies.
2. Explain how `str.compareTo()` calculates lexicographical difference character-by-character.

---

## 14. Real-World Applications

- **Text Processing & Compilers:** Parsing programming source code, tokenization, syntax highlighting.
- **Web Search Engines:** Inverted indexing, fuzzy text matching, auto-complete suggestions.
- **JSON / XML Serialization:** Building formatted data payloads using fast string buffers.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`CharAtDemo.java`](CharAtDemo.java) | Accessing individual characters in a string by index using `str.charAt()`. |
| [`LargestStringLexicographical.java`](LargestStringLexicographical.java) | Finding lexicographically largest string in an array using `compareTo()`. |
| [`PalindromeStringCheck.java`](PalindromeStringCheck.java) | Two-pointer palindrome check comparing symmetrical characters. |
| [`StringBasics.java`](StringBasics.java) | String declaration, Scanner input (`next()` vs `nextLine()`), and basic concepts. |
| [`StringBuilderDemo.java`](StringBuilderDemo.java) | Efficient $O(N)$ string appending using mutable `StringBuilder`. |
| [`StringCompression.java`](StringCompression.java) | Run-length string compression algorithm aggregating repeating character counts. |
| [`StringConcatenation.java`](StringConcatenation.java) | Basic string concatenation using the `+` operator. |
| [`StringEqualityDemo.java`](StringEqualityDemo.java) | Deep dive into `==` vs `.equals()` and String Constant Pool behavior. |
| [`StringLengthDemo.java`](StringLengthDemo.java) | Measuring character count in strings using `str.length()`. |
| [`SubstringDemo.java`](SubstringDemo.java) | Extracting substrings using custom loop vs built-in `str.substring(si, ei)`. |
| [`ToUpperCaseWords.java`](ToUpperCaseWords.java) | Capitalizing the first letter of every word in a sentence using `StringBuilder`. |

---

## 16. Related Topics

### Prerequisites
- Java Basics & 1D Arrays.

### Related Topics
- Pattern Matching Algorithms (KMP, Rabin-Karp).
- Bit Manipulation (character ASCII bit tricks).

### Next Topics
- Recursion & String Permutations / Subsets.
- Tries & Prefix Trees.

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add visual notes for String Constant Pool layout or StringBuilder capacity growth here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
