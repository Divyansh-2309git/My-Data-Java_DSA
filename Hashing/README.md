# Hashing (HashMap, HashSet & Custom Implementations)

## 1. What Is This?

Hashing is a technique used to uniquely identify objects and map keys to specific array indices using a mathematical function called a **Hash Function**.

Hashing provides average **$O(1)$ constant time** for search, insertion, and deletion operations, making it one of the most powerful and widely used concepts in software engineering.

This module covers custom `HashMap` implementations, collision handling, rehashing mechanics, Java Collection Map/Set variants (`HashMap`, `LinkedHashMap`, `TreeMap`, `HashSet`, `LinkedHashSet`, `TreeSet`), and classical hashing algorithmic patterns.

---

## 2. Core Idea

### Key-Value Storage & Separate Chaining

1. **Hash Function:** Converts a Key $K$ into a bucket index $i$:
   $$\text{Bucket Index } i = |\text{key.hashCode()}| \pmod{\text{Number of Buckets}}$$
2. **Collision:** Occurs when two distinct keys yield the exact same bucket index ($i_1 == i_2$).
3. **Separate Chaining:** Resolves collisions by storing a Linked List of nodes at each bucket index.

```text
Buckets Array (Size N = 4)
  Index 0 ──► null
  Index 1 ──► [ "India" : 150 ] ──► [ "China" : 140 ] ──► null  (Chaining!)
  Index 2 ──► [ "US" : 330 ] ──► null
  Index 3 ──► null
```

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Hash Function** | Function mapping input keys to integer array indices. |
| **Collision** | Situation where two different keys map to the same bucket index. |
| **Separate Chaining** | Collision resolution storing a LinkedList of key-value nodes inside each bucket. |
| **Load Factor ($\lambda$)** | Ratio of total entries $N$ to bucket count $K$ ($\lambda = N / K$). |
| **Rehashing** | Doubling bucket array size ($2K$) and re-inserting all nodes when $\lambda > \text{threshold}$ (e.g. 2.0). |
| **`entrySet()`** | Set of key-value pair entries used to iterate through a `HashMap`. |
| **`keySet()`** | Set of all keys present in a `HashMap`. |

---

## 4. Visual / Mental Model

### Load Factor & Rehashing Mechanics

Suppose bucket array size $K = 4$. We insert 9 key-value pairs ($N = 9$).

$$\text{Load Factor } \lambda = \frac{N}{K} = \frac{9}{4} = 2.25 > 2.0 \text{ (Threshold Exceeded!)}$$

```text
  Old Buckets (K = 4, Long Chains, Slower O(N) lookup!)
  [0] ──► [A] ──► [B] ──► [C]
  [1] ──► [D] ──► [E]
  [2] ──► [F] ──► [G] ──► [H] ──► [I]
                     │
                     ▼  REHASHING TRIGGERED! (Allocate K = 8 buckets)
  New Buckets (K = 8, Short Chains, Restores fast O(1) lookup!)
  [0] ──► [A]
  [1] ──► [D]
  [2] ──► [F]
  [3] ──► [B] ──► [E]
  ...
```

---

## 5. Operations & Collections Comparison

### Map Collections Comparison

| Map Variant | Internal Structure | Ordering | Time Complexity | Allows Null Keys? |
|---|---|---|:---:|:---:|
| **`HashMap`** | Buckets Array + LinkedList / Red-Black Tree | Unordered | Average $O(1)$ | Yes (1 null key) |
| **`LinkedHashMap`** | HashMap + Doubly-Linked Buckets | Insertion Order | $O(1)$ | Yes |
| **`TreeMap`** | Red-Black Self-Balancing BST | Sorted Keys | $O(\log N)$ | No |

### Set Collections Comparison

| Set Variant | Internal Structure | Ordering | Time Complexity | Unique Elements? |
|---|---|---|:---:|:---:|
| **`HashSet`** | Backed by `HashMap` | Unordered | Average $O(1)$ | Yes |
| **`LinkedHashSet`** | Backed by `LinkedHashMap` | Insertion Order | $O(1)$ | Yes |
| **`TreeSet`** | Backed by `TreeMap` | Sorted Order | $O(\log N)$ | Yes |

---

## 6. Worked Examples

### Worked Example: Largest Subarray with Sum 0 (Prefix Sum + HashMap)

**Input Array:** `[15, -2, 2, -8, 1, 7, 10, 23]`

- **Algorithm:** Store `(prefixSum, index)` in HashMap. If a prefix sum repeats at index $j$ after index $i$, the subarray sum between $i+1$ and $j$ MUST be **0**!
  $$\text{Subarray Sum } (i+1 \dots j) = \text{prefixSum}[j] - \text{prefixSum}[i] = 0$$

- **Step-by-Step Trace:**

| Index ($j$) | Value | Prefix Sum | Map State `(sum -> first_idx)` | Subarray Length | Max Length |
|:---:|:---:|:---:|---|:---:|:---:|
| -1 | - | 0 | `{(0 -> -1)}` | - | 0 |
| 0 | 15 | 15 | `{(0 -> -1), (15 -> 0)}` | - | 0 |
| 1 | -2 | 13 | `... (13 -> 1)` | - | 0 |
| 2 | 2 | 15 | 15 SEEN AT INDEX 0! | $2 - 0 = 2$ | **2** |
| 3 | -8 | 7 | `... (7 -> 3)` | - | 2 |
| 4 | 1 | 8 | `... (8 -> 4)` | - | 2 |
| 5 | 7 | 15 | 15 SEEN AT INDEX 0! | $5 - 0 = 5$ | **5** |
| 6 | 10 | 25 | `... (25 -> 6)` | - | 5 |
| 7 | 23 | 48 | `... (48 -> 7)` | - | 5 |

- **Result:** **Max Subarray Length = `5`** (Subarray `[-2, 2, -8, 1, 7]`).

---

## 7. Java Implementation Concepts

- **Custom HashMap Implementation Architecture (`ImplementationHashMaps.java`):**
  ```java
  public class CustomHashMap<K, V> {
      private class Node {
          K key; V value;
          public Node(K key, V value) { this.key = key; this.value = value; }
      }
      private int n; // Total nodes
      private int N; // Total buckets
      private LinkedList<Node> buckets[];
      
      @SuppressWarnings("unchecked")
      public CustomHashMap() {
          this.N = 4;
          this.buckets = new LinkedList[4];
          for (int i = 0; i < 4; i++) buckets[i] = new LinkedList<>();
      }
  }
  ```
- **HashMap Iteration Modes:**
  ```java
  // 1. Key Set Iteration
  for (String key : map.keySet()) {
      System.out.println(key + " -> " + map.get(key));
  }
  
  // 2. Entry Set Iteration (Faster!)
  for (Map.Entry<String, Integer> entry : map.entrySet()) {
      System.out.println(entry.getKey() + " -> " + entry.getValue());
  }
  ```

---

## 8. Problem-Solving Patterns

### Pattern 1: Frequency Count Map
- **When to think of it:** Majority Element, Valid Anagram, Top K Frequent Elements.
- **Mental Approach:** Use `map.put(val, map.getOrDefault(val, 0) + 1)` to count occurrences in $O(N)$ time.

### Pattern 2: Prefix Sum + HashMap Index Lookup
- **When to think of it:** Subarray sum equals K, Largest subarray with sum 0.
- **Mental Approach:** Maintain running `prefixSum`. Store first occurrence index of each prefix sum in map.

### Pattern 3: Seen Set for Uniqueness
- **When to think of it:** Count distinct elements, Contains duplicate, Union & Intersection of arrays.
- **Mental Approach:** Insert elements into `HashSet`. Set automatically ignores duplicate additions!

---

## 9. Algorithms

### Custom HashMap Rehashing Algorithm
- **Pseudocode:**
  ```text
  function rehash():
      oldBuckets = buckets
      buckets = new LinkedList[2 * N]
      N = 2 * N
      for i = 0 to N - 1: buckets[i] = new LinkedList()
      
      for bucket in oldBuckets:
          for node in bucket:
              put(node.key, node.value) // Re-inserts at new hash indices!
  ```
- **Time Complexity:** $O(N)$ | **Space Complexity:** $O(N)$

---

## 10. Complexity Reference

| Data Structure | Search | Insert | Delete | Ordering |
|---|:---:|:---:|:---:|---|
| **`HashMap`** | Average $O(1)$ | Average $O(1)$ | Average $O(1)$ | Unordered |
| **`LinkedHashMap`** | $O(1)$ | $O(1)$ | $O(1)$ | Insertion Order |
| **`TreeMap`** | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | Sorted Key Order |
| **`HashSet`** | Average $O(1)$ | Average $O(1)$ | Average $O(1)$ | Unordered |
| **`LinkedHashSet`** | $O(1)$ | $O(1)$ | $O(1)$ | Insertion Order |
| **`TreeSet`** | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | Sorted Order |

---

## 11. Common Mistakes

- **Forgetting `getOrDefault()`:** Writing `map.put(key, map.get(key) + 1)` without checking `containsKey(key)` throws `NullPointerException` on unseen keys! Always use `map.getOrDefault(key, 0) + 1`.
- **Modifying Key Objects After Insertion:** Mutating an object after inserting it as a key alters its `hashCode()`, making the key unfindable in the map!
- **Overwriting Subarray First Occurrence in Sum 0:** Updating `map.put(sum, i)` when `sum` ALREADY exists in the map reduces the window size! ONLY insert `sum` if `!map.containsKey(sum)` to preserve the maximum left boundary!

---

## 12. Edge Cases

- **Subarray Sum Starting at Index 0:** Initializing `map.put(0, -1)` is MANDATORY to detect sum 0 subarrays that include index 0!
- **Null Keys in `TreeMap` / `TreeSet`:** Throws `NullPointerException` because sorted comparison (`compareTo`) fails on `null`.
- **High Collision Keys:** Bad hash functions mapping all keys to 1 bucket (degrading time to $O(N)$). Java 8+ converts buckets with $> 8$ nodes into Red-Black Trees ($O(\log N)$ worst-case).

---

## 13. Interview Questions

### Beginner
1. How does a `HashMap` achieve average $O(1)$ lookup time complexity?
2. Explain the difference between `HashMap`, `LinkedHashMap`, and `TreeMap` in Java.

### Intermediate
1. Explain Separate Chaining, Load Factor ($\lambda$), and the Rehashing process in custom HashMap design.
2. How do you find the Largest Subarray with Sum 0 in $O(N)$ time using Prefix Sums and a HashMap?

### Advanced
1. Explain how Java 8+ optimizes HashMap collision resolution by transforming long LinkedList chains into Red-Black Trees when bucket size exceeds 8 nodes.
2. Design a data structure for an Itinerary Planner that reconstructs travel paths from unordered ticket pairs `(From -> To)` in $O(N)$ time.

---

## 14. Real-World Applications

- **Database Indexing & Caching:** In-memory key-value caches like Redis and Memcached.
- **Web Routers & Session Stores:** Storing user HTTP cookies and session authentication tokens.
- **Compiler Symbol Tables:** Storing variable declarations, types, and scope memory mappings.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`HashMaps.java`](HashMaps.java) | Basic `HashMap` operations (`put`, `get`, `containsKey`, `remove`, `size`, `clear`). |
| [`IterationOnHashMaps.java`](IterationOnHashMaps.java) | Iterating through `HashMap` using `keySet()` and `entrySet()`. |
| [`ImplementationHashMaps.java`](ImplementationHashMaps.java) | Complete custom generic `HashMap<K,V>` class with array of LinkedList buckets, hash function, separate chaining, and automatic $O(N)$ rehashing. |
| [`LinkedHashMaps.java`](LinkedHashMaps.java) | `LinkedHashMap` insertion-order preservation demonstration. |
| [`TreeMaps.java`](TreeMaps.java) | `TreeMap` sorted key-value storage demonstration ($O(\log N)$ operations). |
| [`HashSets.java`](HashSets.java) | `HashSet` operations for unique element storage (`add`, `contains`, `remove`). |
| [`IterationOnHashSets.java`](IterationOnHashSets.java) | Iterating over `HashSet` using Iterators and enhanced `for` loops. |
| [`LinkedHashSets.java`](LinkedHashSets.java) | `LinkedHashSet` maintaining insertion order for distinct elements. |
| [`TreeSets.java`](TreeSets.java) | `TreeSet` maintaining sorted order for unique elements. |
| [`MajorityElement.java`](MajorityElement.java) | Frequency counting algorithm finding elements appearing more than $\lfloor N/3 \rfloor$ times. |
| [`ValidAnagram.java`](ValidAnagram.java) | Anagram check using frequency counting map in $O(N)$ time. |
| [`CountDistinctElements.java`](CountDistinctElements.java) | Counting unique numbers in an array using `HashSet` in $O(N)$ time. |
| [`UnionAndIntersection.java`](UnionAndIntersection.java) | Computing Set Union and Set Intersection of two arrays using `HashSet`. |
| [`ItineraryForTickets.java`](ItineraryForTickets.java) | Finding travel itinerary from unsorted ticket pairs by detecting starting point with in-degree 0 in reversed map. |
| [`LargestSubarrayWithSum0.java`](LargestSubarrayWithSum0.java) | Finding largest contiguous subarray with sum 0 using Prefix Sum + HashMap in $O(N)$ time. |

---

## 16. Related Topics

### Prerequisites
- Arrays, Singly Linked Lists, & Objects (`hashCode` / `equals`).

### Related Topics
- Heaps & Priority Queues.
- Trie & Prefix Trees.

### Next Topics
- Graph Algorithms (Adjacency List maps).
- Dynamic Programming & Caching (Memoization maps).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for Separate Chaining buckets or Subarray Sum 0 prefix map trace here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
