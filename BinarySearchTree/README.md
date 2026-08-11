# Binary Search Trees (BST)

## 1. What Is This?

A Binary Search Tree (BST) is a specialized binary tree data structure designed for ultra-fast searching, insertion, and deletion operations.

A binary tree is a valid BST if and only if it satisfies the **BST Property** at every single node:

$$\text{All nodes in Left Subtree} < \text{Root Node} < \text{All nodes in Right Subtree}$$

Because of this ordering constraint, an **Inorder Traversal (Left $\to$ Root $\to$ Right) of a BST ALWAYS yields a strictly sorted array in ascending order**!

---

## 2. Core Idea

```text
                     8          <-- ROOT NODE
                   /   \
                  3     10      <-- Left < 8 < Right
                 / \      \
                1   6      14   <-- Left < Parent < Right
                   / \     /
                  4   7   13
```

- To **Search** for a value $K$:
  - If $K == \text{node.data}$: Found!
  - If $K < \text{node.data}$: Search **Left Subtree**.
  - If $K > \text{node.data}$: Search **Right Subtree**.

This eliminates half of the remaining search space at every step (similar to Binary Search on arrays!), running in $O(H)$ time where $H$ is tree height.

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **BST Property** | $\text{Left} < \text{Node} < \text{Right}$ condition for all subtrees. |
| **Inorder Traversal** | Visiting Left $\to$ Root $\to$ Right. Output is ALWAYS sorted in ascending order! |
| **Inorder Successor** | The node with the smallest key strictly greater than current node (minimum value in right subtree). |
| **Balanced BST** | A BST where height is maintained at $H = O(\log N)$ (e.g. AVL, Red-Black Trees). |
| **Skewed BST** | Degenerate line-graph tree where height $H = N$ (worst-case $O(N)$ operations). |

---

## 4. Visual / Mental Model

### Deletion in BST (Case 3: Node with Two Children)

Suppose we want to delete node `5` which has two children:

```text
         8                         8
       /   \                     /   \
     [5]   10  ───► Replace ──► 6     10
     / \           val with     / \
    3   6          Inorder     3   (deleted!)
       /           Successor
      6            (min of right)
```

1. Find Inorder Successor (min node in right subtree $\to$ `6`).
2. Copy Inorder Successor's value into node `5`.
3. Delete original Inorder Successor node from right subtree (which has at most 1 child!).

---

## 5. Operations / Techniques

### 1. Insertion ($O(H)$)
Recursively compare key with current node. Insert as a leaf when hitting `null`.

### 2. Search ($O(H)$)
Compare key and recurse down left or right child branch.

### 3. Deletion ($O(H)$)
- **Case 1 (Leaf Node):** Return `null` to parent.
- **Case 2 (Single Child):** Return the non-null child to parent.
- **Case 3 (Two Children):** Replace data with Inorder Successor (`findMin(root.right)`), then recursively delete Inorder Successor from right subtree.

### 4. Print in Range `[k1, k2]`
- If `root.data >= k1 && root.data <= k2`: Recurse left, print root, recurse right.
- If `root.data > k2`: Recurse left ONLY (pruning right subtree!).
- If `root.data < k1`: Recurse right ONLY (pruning left subtree!).

### 5. Validate BST ($O(N)$ Range Limits)
Validate node data is strictly bounded within $(\text{min}, \text{max})$ limits: `isValidBST(root, min, max)`.

---

## 6. Worked Examples

### Worked Example: Largest BST in Binary Tree ($O(N)$ Bottom-Up)

**Given Binary Tree:**

```text
       50
     /    \
   30      60
  /  \    /  \
 5   20  45   70
             /  \
            65   80
```

- **Step 1:** Leaves `5`, `20` return `isBST = true, size = 1`.
- **Step 2:** Subtree `30`: Left is `5` ($< 30$), Right is `20` ($< 30$ $\implies$ **INVALID BST!** $20 < 30$).
  - Returns `isBST = false, size = max(1, 1) = 1`.
- **Step 3:** Right Subtree `60`:
  - Subtree `70` with children `65, 80` is a **VALID BST** of size 3 (`min=65, max=80`).
  - Node `60` with left child `45` and right child `70` (`min=45 < 60 < max=70`) is a **VALID BST** of size 5 (`min=45, max=80`).
- **Step 4:** Root `50`: Left child `30` is NOT a valid BST.
- **Result:** **Largest BST Subtree Size = `5`** (rooted at node `60`).

---

## 7. Java Implementation Concepts

- **Valid BST Range Guard:** Always pass `Node min, Node max` references to handle negative/large integer limits:
  ```java
  public static boolean isValidBST(Node root, Node min, Node max) {
      if (root == null) return true;
      if (min != null && root.data <= min.data) return false;
      if (max != null && root.data >= max.data) return false;
      return isValidBST(root.left, min, root) && isValidBST(root.right, root, max);
  }
  ```

---

## 8. Problem-Solving Patterns

### Pattern 1: Inorder Traversal Property
- **When to think of it:** Validating BST, finding $K$-th smallest/largest element, converting BST to sorted array.
- **Mental Approach:** Perform Inorder DFS. The resulting values MUST be strictly increasing.

### Pattern 2: Range Limit Boundaries
- **When to think of it:** Printing values in range `[L, R]`, validating BST property.
- **Mental Approach:** Pass allowed lower bound `min` and upper bound `max` down recursion branches.

---

## 9. Algorithms

### Deletion in BST Algorithm
- **Pseudocode:**
  ```text
  function delete(root, val):
      if root == null return null
      if val < root.data: root.left = delete(root.left, val)
      else if val > root.data: root.right = delete(root.right, val)
      else: // Found node to delete!
          if root.left == null and root.right == null: return null
          if root.left == null: return root.right
          if root.right == null: return root.left
          IS = findMin(root.right) // Inorder Successor
          root.data = IS.data
          root.right = delete(root.right, IS.data)
      return root
  ```
- **Time Complexity:** $O(H)$ ($O(\log N)$ average, $O(N)$ worst) | **Space:** $O(H)$

---

## 10. Complexity Reference

| Operation | Average Case (Balanced) | Worst Case (Skewed) | Auxiliary Space |
|---|:---:|:---:|:---:|
| Search | $O(\log N)$ | $O(N)$ | $O(H)$ |
| Insertion | $O(\log N)$ | $O(N)$ | $O(H)$ |
| Deletion | $O(\log N)$ | $O(N)$ | $O(H)$ |
| Validate BST | $O(N)$ | $O(N)$ | $O(H)$ |
| Print in Range | $O(H + K)$ | $O(N)$ | $O(H)$ |
| Largest BST Subtree | $O(N)$ | $O(N)$ | $O(H)$ |

---

## 11. Common Mistakes

- **Checking Only Immediate Children for BST Validation:** Testing `node.left.data < node.data` is INSUFFICIENT! Every node in the ENTIRE left subtree must be less than root. Always use **Range Limits** (`min` and `max`).
- **Losing Pointer Links on Deletion:** Forgetting to return updated `root` in deletion recursion (`root.left = delete(root.left, val)`).
- **Selecting Wrong Inorder Successor:** Inorder successor is the MINIMUM node in the RIGHT subtree (keep moving `left` in right child), NOT just `root.right`!

---

## 12. Edge Cases

- **Empty BST (`root == null`).**
- **Single Node BST.**
- **Skewed BST (Degenerate linked-list line graph).**
- **Deleting Root Node of BST.**
- **Range `[k1, k2]` with no matching values in tree.**

---

## 13. Interview Questions

### Beginner
1. What is the defining property of a Binary Search Tree (BST)?
2. What order do elements appear in when performing an Inorder Traversal on a BST?

### Intermediate
1. Explain the three cases for deleting a node from a BST (especially two-children deletion).
2. How do you validate whether a given Binary Tree is a valid BST in $O(N)$ time?

### Advanced
1. How do you find the size of the Largest BST Subtree inside a general Binary Tree in $O(N)$ time?
2. Compare unbalanced BST ($O(N)$ worst case) vs self-balancing BSTs (AVL / Red-Black Trees guaranteeing $O(\log N)$).

---

## 14. Real-World Applications

- **Database Indexing:** B-Trees and Red-Black Trees store database indices for rapid logarithmic key lookups.
- **Symbol Tables & Sets:** Implementing sorted sets (`java.util.TreeSet`) and sorted maps (`java.util.TreeMap`).
- **Priority Range Queries:** Quick retrieval of elements within specific min-max bounds.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`BSTBasics.java`](BSTBasics.java) | Comprehensive BST implementation featuring `insert`, `search`, `delete`, `inorder`, `printInRange`, `printRoot2Leaf`, and `isValidBST`. |
| [`LargestBST.java`](LargestBST.java) | Bottom-up $O(N)$ algorithm finding the size of the largest valid BST subtree within a general Binary Tree. |

---

## 16. Related Topics

### Prerequisites
- Binary Trees & Depth-First Traversals (DFS).

### Related Topics
- Heaps & Priority Queues.
- Red-Black Trees & AVL Trees.

### Next Topics
- Heaps & Priority Queues.
- Hashing (HashMap & HashSet).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for BST Deletion cases or Inorder Traversal output trace here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
