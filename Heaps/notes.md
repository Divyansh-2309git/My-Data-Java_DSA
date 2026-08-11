# Heap Array Indexing Formulas & Mental Model

![formula for finding the left child and the right child of the parent in a array / arraylist. ](image.png)

## Array Representation of Complete Binary Trees

A Heap is conceptually a Complete Binary Tree, but physically stored in a contiguous 1D `ArrayList` or Array.

For any element at 0-based index `i`:

| Relationship | Index Formula |
|---|---|
| **Parent Index** | `(i - 1) / 2` |
| **Left Child Index** | `2 * i + 1` |
| **Right Child Index** | `2 * i + 2` |

---

## Example Walkthrough

Array representation: `[10, 20, 30, 40, 50]`

- Index 0 (`10`): Left child at `2(0)+1 = 1` (`20`), Right child at `2(0)+2 = 2` (`30`).
- Index 1 (`20`): Parent at `(1-1)/2 = 0` (`10`). Left child at `2(1)+1 = 3` (`40`), Right child at `2(1)+2 = 4` (`50`).
- Index 2 (`30`): Parent at `(2-1)/2 = 0` (`10`).
