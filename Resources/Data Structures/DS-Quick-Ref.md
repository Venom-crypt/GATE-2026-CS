# Programming and Data Structures - GATE 2026 Quick Reference

## Pointer Basics

| Concept | Usage | Example |
|---------|-------|---------|
| **Address-of (&)** | Get address of variable | &x |
| **Dereference (*)** | Access value at address | *ptr |
| **Pointer arithmetic** | Move in memory | ptr++, ptr+5 |
| **Pointer to pointer** | Multi-level indirection | int **pptr |
| **NULL pointer** | Invalid/uninitialized | ptr = NULL |

**Pointer Size:**
- 32-bit system: 4 bytes (all pointers)
- 64-bit system: 8 bytes (all pointers)

---

## Memory Management in C

| Function | Purpose | Example |
|----------|---------|---------|
| **malloc** | Allocate memory | `malloc(10 * sizeof(int))` |
| **calloc** | Allocate + initialize to 0 | `calloc(10, sizeof(int))` |
| **realloc** | Resize allocated memory | `realloc(ptr, new_size)` |
| **free** | Deallocate memory | `free(ptr); ptr=NULL;` |

**Memory Regions (lowest to highest):**
1. Code segment (instructions)
2. Data segment (global variables)
3. Heap (dynamic allocation, grows upward)
4. Stack (local variables, grows downward)

---

## Recursion Components

**Base Case:** Stops recursion (must exist!)
**Recursive Case:** Calls itself with smaller input
**Call Stack:** Each call creates frame with local variables, parameters, return address

**Stack Overflow:** Too many recursive calls → crash

---

## Array vs Linked List

| Operation | Array | Linked List |
|-----------|-------|-------------|
| Access | O(1) | O(n) |
| Search | O(n) | O(n) |
| Insert (position known) | O(n) | O(1) |
| Insert (at end) | O(1) | O(n) if no tail |
| Delete | O(n) | O(1) if node known |
| Space | O(n) | O(n) + pointers |

---

## Stack and Queue

### Stack (LIFO - Last In First Out)
```
Operations: Push, Pop, Peek - all O(1)
Uses: Function calls, undo/redo, backtracking
```

### Queue (FIFO - First In First Out)
```
Operations: Enqueue, Dequeue, Peek - all O(1)
Uses: BFS, task scheduling, message queues
```

**Circular Queue:** Wrap around using modulo
```
front = (front + 1) % capacity
rear = (rear + 1) % capacity
```

---

## Tree Traversals

### Depth-First (DFS) Traversals

**Inorder (Left-Root-Right):** Visit root between subtrees
- BST inorder = sorted order
- Example: `inorder(left), visit(root), inorder(right)`

**Preorder (Root-Left-Right):** Visit root first
- Prefix expression
- Tree copy
- Example: `visit(root), preorder(left), preorder(right)`

**Postorder (Left-Right-Root):** Visit root last
- Delete tree safely
- Postfix expression
- Example: `postorder(left), postorder(right), visit(root)`

### Breadth-First Traversal (Level-order)
```
Use queue, process level by level
```

---

## Binary Search Tree (BST)

### Properties
- Left subtree: all values < root
- Right subtree: all values > root
- Both subtrees are BSTs

### Operations Time Complexity
| Operation | Average | Worst |
|-----------|---------|-------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

### Deletion Cases
1. **No children (leaf):** Remove directly
2. **One child:** Replace with child
3. **Two children:** Replace with inorder successor (smallest in right subtree) or predecessor (largest in left subtree)

### Balanced BSTs
- **AVL Tree:** Height difference ≤ 1, O(log n) guaranteed
- **Red-Black Tree:** Color-based, O(log n) guaranteed

---

## Binary Heaps

### Types
**Max-Heap:** Parent ≥ Children (max at root)
**Min-Heap:** Parent ≤ Children (min at root)

### Array Representation (0-indexed)
```
Node at index i:
- Left child: 2i + 1
- Right child: 2i + 2
- Parent: (i-1)/2
```

### Operations
- **Insert:** Add at end, heapify up, O(log n)
- **Delete Min/Max:** Remove root, move last to root, heapify down, O(log n)
- **Build Heap:** O(n)
- **Heap Sort:** O(n log n)

### Applications
- Priority queues
- Dijkstra's algorithm
- Huffman coding

---

## Linked List Types

| Type | Structure | Traversal | Insert |
|------|-----------|-----------|--------|
| **Singly** | Data + next | Forward only | O(n) |
| **Doubly** | Data + next + prev | Both ways | O(n) |
| **Circular** | Last → first | Loop forever | O(n) |

---

## Graph Representation

### Adjacency Matrix
```
- O(1) edge lookup
- O(V²) space
- Good for dense graphs
```

### Adjacency List
```
- O(V+E) space
- Better for sparse graphs
- Variable edges per node
```

---

## Common Data Structure Complexities

| DS | Access | Search | Insert | Delete |
|----|--------|--------|--------|--------|
| Array | O(1) | O(n) | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1)* | O(1)* |
| Stack | O(1) | N/A | O(1) | O(1) |
| Queue | O(1) | N/A | O(1) | O(1) |
| BST | O(log n) | O(log n) | O(log n) | O(log n) |
| Heap | O(1) root | O(n) | O(log n) | O(log n) |

*If position known

---

## Recursion vs Iteration

| Property | Recursion | Iteration |
|----------|-----------|-----------|
| Code | Simpler, cleaner | More complex |
| Memory | Stack frames | Minimal |
| Speed | Slower (overhead) | Faster |
| Depth | Limited by stack | No limit |
| Backtracking | Natural | Needs explicit |

---

## GATE Question Patterns

**Pattern 1:** Trace output of recursive function
- Track call stack, values at each level

**Pattern 2:** Identify data structure
- Given operations → determine DS

**Pattern 3:** Complexity of tree operation
- Balanced: O(log n), Unbalanced: O(n)

**Pattern 4:** BST properties
- In-order traversal = sorted
- Search/Insert/Delete = O(log n) avg

**Pattern 5:** Memory management
- Pointer arithmetic, malloc/free

**Pattern 6:** Linked list operations
- Insert/delete specific position

**Pattern 7:** Tree traversal order
- Given tree, identify in/pre/post-order

---

## Common Mistakes

✗ Forgetting base case in recursion → infinite loop
✗ Confused about pointer arithmetic (size-based)
✗ Not freeing allocated memory → memory leak
✗ Accessing array out of bounds
✗ Confusing LIFO (stack) and FIFO (queue)
✗ BST deletion with 2 children (must use successor/predecessor)
✗ Wrong heap index formula
✗ Tree traversal wrong order

---

## C Program Template

```c
#include <stdio.h>
#include <stdlib.h>

// Function prototypes
void function();

// Main
int main() {
    // Memory allocation
    int *arr = (int *)malloc(n * sizeof(int));
    
    // Code
    
    // Free memory
    free(arr);
    arr = NULL;
    
    return 0;
}

// Function implementation
void function() {
    // Code
}
```

---

**Print this sheet for exam day!**
