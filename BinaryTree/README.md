# Binary Trees & Tree Metrics

## 1. What Is This?

A Binary Tree is a non-linear hierarchical data structure composed of nodes, where each node contains a data payload and at most two child references called **LEFT** and **RIGHT**.

Unlike linear data structures (Arrays, Linked Lists, Stacks, Queues) where elements follow a sequential order, trees represent hierarchical relationships such as file directories, organizational charts, XML/DOM documents, and decision trees.

---

## 2. Core Idea

```text
                     10         <-- ROOT NODE
                   /    \
                  20     30     <-- PARENT / CHILD NODES
                 /  \      \
                40  50     60   <-- LEAF NODES (left == null, right == null)
```

1. **Root:** The topmost node in the tree with no parent.
2. **Leaf Node:** A node that has no children (`left == null` and `right == null`).
3. **Subtree:** Any node together with all of its descendants forms a valid binary tree.

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Height of Tree** | Distance (number of nodes/edges) on the longest path from root to a leaf node. |
| **Depth of Node** | Distance from root node down to that specific node. |
| **Preorder Traversal** | Root $\to$ Left Subtree $\to$ Right Subtree (DFS). |
| **Inorder Traversal** | Left Subtree $\to$ Root $\to$ Right Subtree (DFS). |
| **Postorder Traversal**| Left Subtree $\to$ Right Subtree $\to$ Root (DFS). |
| **Level Order Traversal**| Traversing tree level-by-level top-to-bottom using a `Queue` (BFS). |
| **Diameter of Tree** | Number of nodes on the longest path between any two leaf nodes in the tree. |

---

## 4. Visual / Mental Model

### Level Order Traversal (BFS Queue Mechanics)

```text
       1
     /   \
    2     3
   / \   / \
  4   5 6   7
```

Queue state progression:
- Push `Root (1)` and `null` (level marker): Queue `[1, null]`
- Pop `1`, print `1`, push children `2, 3`: Queue `[null, 2, 3]`
- Pop `null` $\implies$ Print newline! Push `null` back: Queue `[2, 3, null]`
- Pop `2`, print `2`, push children `4, 5`: Queue `[3, null, 4, 5]`
- Pop `3`, print `3`, push children `6, 7`: Queue `[null, 4, 5, 6, 7]`

Output printed level-by-level:
```text
1
2 3
4 5 6 7
```

---

## 5. Operations / Techniques

### 1. Building Tree from Preorder Sequence
Given array `nodes = [1, 2, 4, -1, -1, 5, -1, -1, 3, -1, 6, -1, -1]`, where `-1` represents a `null` pointer.
Construct tree recursively using static index tracking.

### 2. Recursive Metrics Calculations (Divide & Conquer on Trees)
- **Height:** `1 + Math.max(height(root.left), height(root.right))`
- **Count of Nodes:** `1 + count(root.left) + count(root.right)`
- **Sum of Nodes:** `root.data + sum(root.left) + sum(root.right)`

### 3. Diameter Optimization ($O(N^2)$ to $O(N)$)
- **Naive Approach ($O(N^2)$):** For every node, compute `height(left) + height(right) + 1`, taking $O(N)$ height calls per node.
- **Optimal Approach ($O(N)$):** Bottom-up recursion returning a `Info` class containing both `height` and `diameter` in a single pass!

---

## 6. Worked Examples

### Worked Example: Diameter of Binary Tree ($O(N)$ Info Class)

```text
       1
     /   \
    2     3
   / \
  4   5
```

- **Step 1:** Base cases for leaves `4` and `5`: `height = 1, diameter = 1`.
- **Step 2:** Node `2`:
  - `leftInfo` from `4`: `(h=1, d=1)`.
  - `rightInfo` from `5`: `(h=1, d=1)`.
  - `selfHeight = 1 + max(1, 1) = 2`.
  - `selfDiameter = max(1 + 1 + 1, max(1, 1)) = 3`.
  - Node `2` returns `(h=2, d=3)`.
- **Step 3:** Node `3` (Leaf): returns `(h=1, d=1)`.
- **Step 4:** Root `1`:
  - `leftInfo` from `2`: `(h=2, d=3)`.
  - `rightInfo` from `3`: `(h=1, d=1)`.
  - `selfHeight = 1 + max(2, 1) = 3`.
  - `selfDiameter = max(hL + hR + 1, max(dL, dR)) = max(2 + 1 + 1, max(3, 1)) = 4`.
- **Result:** **Diameter = `4`** (Longest path `4 -> 2 -> 1 -> 3` contains 4 nodes).

---

## 7. Java Implementation Concepts

- **Static Node Definition:**
  ```java
  public static class Node {
      int data;
      Node left;
      Node right;
      public Node(int data) {
          this.data = data;
          this.left = null;
          this.right = null;
      }
  }
  ```
- **Helper Info Class for Bottom-Up Return:**
  ```java
  static class Info {
      int height;
      int diameter;
      public Info(int height, int diameter) {
          this.height = height;
          this.diameter = diameter;
      }
  }
  ```

---

## 8. Problem-Solving Patterns

### Pattern 1: Depth-First Recursive Traversal
- **When to think of it:** Height, Node counting, Preorder/Inorder/Postorder traversals.
- **Mental Approach:** Compute result for left subtree, compute result for right subtree, combine at root.

### Pattern 2: Level-Order Queue Traversal (BFS)
- **When to think of it:** Level-by-level printing, minimum depth of binary tree, finding rightmost node at each level.
- **Mental Approach:** Initialize `Queue<Node> q`. Push root and level separator `null`.

---

## 9. Algorithms

### $O(N)$ Optimized Diameter Algorithm
- **Pseudocode:**
  ```text
  function getDiameter(root):
      if root == null: return Info(height = 0, diameter = 0)
      
      leftInfo = getDiameter(root.left)
      rightInfo = getDiameter(root.right)
      
      selfHeight = 1 + max(leftInfo.height, rightInfo.height)
      pathThroughRoot = leftInfo.height + rightInfo.height + 1
      selfDiameter = max(pathThroughRoot, max(leftInfo.diameter, rightInfo.diameter))
      
      return Info(selfHeight, selfDiameter)
  ```
- **Time Complexity:** $O(N)$ | **Space Complexity:** $O(H)$ where $H$ is tree height

---

## 10. Complexity Reference

| Operation / Algorithm | Time Complexity | Auxiliary Space |
|---|---|---|
| Build Tree from Preorder | $O(N)$ | $O(N)$ call stack |
| Preorder / Inorder / Postorder | $O(N)$ | $O(H)$ call stack |
| Level Order Traversal (BFS) | $O(N)$ | $O(W)$ max level width |
| Tree Height Calculation | $O(N)$ | $O(H)$ call stack |
| Total Node Count / Sum | $O(N)$ | $O(H)$ call stack |
| Naive Diameter ($O(N^2)$) | $O(N^2)$ | $O(H)$ call stack |
| Optimized Diameter ($O(N)$) | $O(N)$ | $O(H)$ call stack |

---

## 11. Common Mistakes

- **Null Pointer Exceptions on Unchecked Subtrees:** Accessing `root.left.data` without checking `root != null` or `root.left != null`.
- **Forgetting Base Case in Tree Recursion:** Failing to handle `if (root == null) return 0;`.
- **Re-calculating Subtree Heights repeatedly:** Computing `height(node)` inside diameter recursion leading to $O(N^2)$ time degradation. Always use bottom-up `Info` class passing!

---

## 12. Edge Cases

- **Empty Tree (`root == null`):** Height = 0, Count = 0, Diameter = 0.
- **Single Node Tree (`root.left == null && root.right == null`):** Height = 1, Diameter = 1.
- **Skewed Binary Tree (Line graph shape):** Height $= N$, recursion call stack depth $= N$.

---

## 13. Interview Questions

### Beginner
1. What is the difference between a Binary Tree and a Binary Search Tree (BST)?
2. Explain Preorder, Inorder, and Postorder DFS traversals with code snippets.

### Intermediate
1. Explain how to perform Level-Order Traversal (BFS) using a Queue and `null` markers.
2. How do you calculate the height and total node count of a Binary Tree recursively?

### Advanced
1. Explain why naive tree diameter calculation takes $O(N^2)$ time and how bottom-up recursion optimizes it to $O(N)$.
2. Write an algorithm to reconstruct a unique Binary Tree given its Inorder and Preorder traversal sequences.

---

## 14. Real-World Applications

- **Abstract Syntax Trees (AST):** Compilers (Java, C++) build ASTs to evaluate expressions and syntax.
- **DOM Trees:** Web browsers render HTML documents as a hierarchical DOM tree.
- **File System Structures:** Directories containing sub-directories and files.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`BuildBinaryTree.java`](BuildBinaryTree.java) | Recursive Binary Tree construction from Preorder sequence with `-1` markers, Preorder, Inorder, Postorder, and Level-Order traversals. |
| [`TreeHeightAndMetrics.java`](TreeHeightAndMetrics.java) | Tree metrics calculations: Height, Node Count, Sum of Nodes, Naive $O(N^2)$ Diameter, and $O(N)$ Optimized Diameter algorithm. |

---

## 16. Related Topics

### Prerequisites
- Recursion Basics, Queues (BFS), and Classes/Nodes.

### Related Topics
- Binary Search Trees (BST).
- Heaps (Complete Binary Trees).

### Next Topics
- Binary Search Tree (BST) operations & queries.
- Graph Algorithms (DFS & BFS generalizations).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for Binary Tree traversals or BFS queue level-order trace here.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
