# Programming and Data Structures Complete Study Guide - GATE 2026

## Part 1: C Programming Fundamentals

### 1.1 Pointers

**Definition:** Variable that stores memory address

**Declaration:**
```c
int *ptr;        // Pointer to int
char *cptr;      // Pointer to char
void *vptr;      // Generic pointer
int **pptr;      // Pointer to pointer
```

**Operators:**
- **& operator:** Address-of operator (get address)
- *** operator:** Dereference (access value)

**Pointer Arithmetic:**
```c
ptr++    // Move to next element (offset by sizeof type)
ptr--    // Move to previous
ptr + 5  // 5 elements ahead
```

**All pointers same size:**
- 32-bit system: 4 bytes
- 64-bit system: 8 bytes

### 1.2 Memory Management

**malloc (Memory Allocation):**
```c
int *arr = malloc(10 * sizeof(int));
```

**calloc (Contiguous Allocation - initializes to 0):**
```c
int *arr = calloc(10, sizeof(int));
```

**realloc (Resize allocated memory):**
```c
int *arr = realloc(arr, 20 * sizeof(int));
```

**free (Deallocate memory):**
```c
free(arr);
arr = NULL;  // Avoid dangling pointer
```

**Memory Regions:**
- **Stack:** Local variables, function calls, returns
- **Heap:** Dynamic allocation, larger
- **Data segment:** Global variables
- **Code segment:** Instructions

### 1.3 Common Issues

**Segmentation Fault:**
- Dereferencing NULL pointer
- Accessing freed memory
- Buffer overflow

**Memory Leak:**
- Allocated but not freed memory
- Result: Resource waste

---

## Part 2: Recursion and Backtracking

### 2.1 Recursion Components

**Base Case:**
- Condition that stops recursion
- Must exist (prevents infinite loop)

**Recursive Case:**
- Function calls itself with simpler input
- Moves toward base case

### 2.2 Call Stack

Each recursive call creates stack frame:
1. Parameters pushed
2. Return address stored
3. Local variables allocated
4. Function executes
5. Stack frame popped on return

**Overflow:** Too many recursive calls fill stack → Crash

### 2.3 Backtracking Pattern

**Choose-Explore-Unchoose:**
```
1. Choose: Pick candidate
2. Explore: Recursively check if valid
3. Unchoose: Undo if path fails, try next
```

**Examples:**
- N-Queens, Permutations, Combinations
- Maze solving, Sudoku

---

## Part 3: Arrays

### 3.1 Array Basics

**Static Array:**
```c
int arr[10];     // Fixed size
int arr[] = {1,2,3};  // Size inferred
```

**Dynamic Array:**
```c
int *arr = malloc(n * sizeof(int));
```

**2D Arrays:**
```c
int arr[3][4];   // Static 3x4
int **arr = malloc(rows * sizeof(int*));  // Dynamic
```

**Time Complexity:**
- Access: O(1)
- Search: O(n)
- Insertion: O(n)
- Deletion: O(n)

---

## Part 4: Stacks

### 4.1 Stack Definition

**LIFO (Last In First Out):** Last element added first removed

**Operations:**
- **Push:** Add element to top, O(1)
- **Pop:** Remove from top, O(1)
- **Peek:** View top without removing, O(1)
- **isEmpty:** Check if empty, O(1)

### 4.2 Stack Implementation

**Using Array:**
```c
struct Stack {
    int *arr;
    int top;
    int capacity;
};
```

**Using Linked List:**
```c
struct Node {
    int data;
    struct Node *next;
};
```

### 4.3 Applications

- Function call management
- Undo/Redo operations
- Expression evaluation
- Backtracking problems

---

## Part 5: Queues

### 5.1 Queue Definition

**FIFO (First In First Out):** First element added first removed

**Operations:**
- **Enqueue:** Add to rear, O(1)
- **Dequeue:** Remove from front, O(1)
- **Peek:** View front, O(1)

### 5.2 Queue Implementation

**Circular Queue:**
```c
front = (front + 1) % capacity
rear = (rear + 1) % capacity
```

**Applications:**
- Task scheduling
- BFS in graphs
- Printer queue
- Message queues

---

## Part 6: Linked Lists

### 6.1 Linked List Structure

**Node:**
```c
struct Node {
    int data;
    struct Node *next;
};
```

**Operations:**
- **Insertion:** O(n) search + O(1) insert = O(n)
- **Deletion:** O(n) search + O(1) delete = O(n)
- **Search:** O(n)

### 6.2 Types

**Singly Linked List:**
- One next pointer
- Unidirectional

**Doubly Linked List:**
- Next and previous pointers
- Bidirectional

**Circular Linked List:**
- Last node points to first
- No NULL pointer

### 6.3 Advantages over Arrays

- Dynamic size
- No contiguous memory needed
- Efficient insertion/deletion at known position
- Disadvantages: No random access, extra memory for pointers

---

## Part 7: Trees

### 7.1 Tree Basics

**Tree:** Connected acyclic graph
- **Root:** Top node (no parent)
- **Leaf:** Node with no children
- **Height:** Longest path from node to leaf
- **Depth:** Distance from root

### 7.2 Binary Trees

**Each node has at most 2 children (left, right)**

**Full Binary Tree:** Every node has 0 or 2 children
**Complete Binary Tree:** All levels filled except possibly last
**Perfect Binary Tree:** All levels completely filled

### 7.3 Tree Traversals

**Depth-First (DFS):**

**Inorder (Left-Root-Right):**
```c
inorder(root->left);
visit(root);
inorder(root->right);
```
Used for: BST → sorted order

**Preorder (Root-Left-Right):**
```c
visit(root);
preorder(root->left);
preorder(root->right);
```
Used for: Tree copy, expression evaluation

**Postorder (Left-Right-Root):**
```c
postorder(root->left);
postorder(root->right);
visit(root);
```
Used for: Delete tree, postfix evaluation

**Breadth-First (Level-order):**
```
Use queue, process level by level
```

---

## Part 8: Binary Search Trees (BST)

### 8.1 BST Property

**For each node:**
- All values in left subtree < node value
- All values in right subtree > node value

### 8.2 BST Operations

**Search:** O(log n) avg, O(n) worst
```
Compare with root:
- Equal: found
- Less: search left
- Greater: search right
```

**Insertion:** O(log n) avg, O(n) worst
```
Find correct position (like search)
Insert as leaf node
```

**Deletion:** O(log n) avg, O(n) worst
- **0 children (leaf):** Remove directly
- **1 child:** Replace with child
- **2 children:** Replace with inorder successor/predecessor

### 8.3 Balanced BST

**AVL Tree:** Height difference ≤ 1
- Operations: O(log n) guaranteed

**Red-Black Tree:** Color-based balancing
- Operations: O(log n) guaranteed

---

## Part 9: Binary Heaps

### 9.1 Heap Property

**Max-Heap:**
- Parent ≥ Children
- Largest at root

**Min-Heap:**
- Parent ≤ Children
- Smallest at root

### 9.2 Array Representation

For node at index i:
- Left child: 2i + 1
- Right child: 2i + 2
- Parent: (i-1)/2

### 9.3 Heap Operations

**Insert:** O(log n)
- Add at end
- Heapify up (bubble)

**Delete Min/Max:** O(log n)
- Remove root
- Replace with last element
- Heapify down (sink)

**Build Heap:** O(n)

### 9.4 Applications

- Priority queues
- Heap sort
- Dijkstra's algorithm
- Huffman coding

---

## Part 10: Graphs

### 10.1 Graph Representation

**Adjacency Matrix:** O(1) edge lookup, O(V²) space
**Adjacency List:** O(V+E) space, better for sparse

### 10.2 Graph Traversals

**BFS:** Breadth-first, level-order
- Uses queue
- O(V+E) time

**DFS:** Depth-first, stack-based
- Uses stack (or recursion)
- O(V+E) time

### 10.3 Applications

- Connected components
- Cycle detection
- Topological sort
- Shortest paths
- MST (Minimum Spanning Tree)

---

## High-Weightage Topics for GATE

1. **Pointers and Memory** (12%)
2. **Trees and BST** (18%)
3. **Linked Lists** (10%)
4. **Stacks and Queues** (12%)
5. **Recursion** (10%)
6. **Arrays** (8%)
7. **Binary Heaps** (10%)
8. **Graphs** (12%)
9. **C Fundamentals** (8%)

**Focus:** Trees/BST, Recursion, Linked Lists, Graph algorithms
