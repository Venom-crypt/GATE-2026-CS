# Algorithms Complete Study Guide - GATE 2026

## Part 1: Searching Algorithms

### 1.1 Linear Search
**Time Complexity:** O(n) - Sequential check each element
**Space Complexity:** O(1)
**Characteristics:** Works on unsorted arrays

### 1.2 Binary Search
**Time Complexity:** O(log n) - Requires sorted array
**Space Complexity:** O(1) iterative, O(log n) recursive
**Precondition:** Array must be sorted

**Algorithm:**
```
Compare middle element with target
If equal: found
If target < middle: search left half
If target > middle: search right half
Repeat until found or range empty
```

---

## Part 2: Sorting Algorithms

### 2.1 Bubble Sort
**Time:** O(n²) worst/avg, O(n) best
**Space:** O(1)
**Stable:** Yes
**Method:** Compare adjacent, swap if needed

### 2.2 Selection Sort
**Time:** O(n²) all cases
**Space:** O(1)
**Stable:** No
**Method:** Find minimum, place at position

### 2.3 Insertion Sort
**Time:** O(n²) worst/avg, O(n) best
**Space:** O(1)
**Stable:** Yes
**Method:** Insert each element into sorted portion

### 2.4 Merge Sort
**Time:** O(n log n) all cases
**Space:** O(n)
**Stable:** Yes
**Method:** Divide → Sort → Merge
**Advantage:** Guaranteed O(n log n)

### 2.5 Quick Sort
**Time:** O(n log n) best/avg, O(n²) worst
**Space:** O(log n) due to recursion
**Stable:** No
**Method:** Partition around pivot, recursive

**Pivot Selection:**
- Random: Avoids worst case
- First/Last: May cause O(n²)
- Median-of-three: Balanced

### 2.6 Heap Sort
**Time:** O(n log n) all cases
**Space:** O(1)
**Stable:** No
**Method:** Build heap → Extract max repeatedly

### 2.7 Counting Sort
**Time:** O(n + k) where k = range
**Space:** O(n + k)
**Stable:** Yes
**Use:** When range is small

### 2.8 Sorting Algorithm Comparison Table

| Algorithm | Best | Avg | Worst | Space | Stable |
|-----------|------|-----|-------|-------|--------|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | ✓ |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | ✗ |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | ✓ |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | ✓ |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | ✗ |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | ✗ |

---

## Part 3: Hashing

### 3.1 Hash Functions
Purpose: Map key to array index
**Requirements:**
- Deterministic: Same key → Same index
- Uniform distribution
- Efficient computation

**Examples:**
- Modulo: h(k) = k mod m
- Multiplication: h(k) = ⌊m(kA mod 1)⌋
- Folding: Divide key into parts, sum

### 3.2 Collision Resolution

**1. Chaining (Open Hashing)**
- Store collisions in linked list
- Average case: O(1 + α) where α = load factor
- Worst case: O(n) if all hash to same value

**2. Open Addressing (Closed Hashing)**
- Find alternative empty slot

**a) Linear Probing:**
- h(k,i) = (h(k) + i) mod m
- Problem: Primary clustering

**b) Quadratic Probing:**
- h(k,i) = (h(k) + c₁i + c₂i²) mod m
- Reduces clustering

**c) Double Hashing:**
- h(k,i) = (h₁(k) + i·h₂(k)) mod m
- Better distribution
- Two hash functions needed

### 3.3 Load Factor
α = n/m (n = elements, m = table size)
- Chaining: Average search O(1 + α)
- Open addressing: Keep α < 0.7

---

## Part 4: Asymptotic Complexity Analysis

### 4.1 Big O Notation (Upper Bound)
f(n) = O(g(n)) if ∃ c, n₀: f(n) ≤ c·g(n) ∀ n ≥ n₀

**Common complexities:**
- O(1) - Constant
- O(log n) - Logarithmic
- O(n) - Linear
- O(n log n) - Linearithmic
- O(n²) - Quadratic
- O(n³) - Cubic
- O(2ⁿ) - Exponential
- O(n!) - Factorial

### 4.2 Big Omega Notation (Lower Bound)
f(n) = Ω(g(n)) if ∃ c, n₀: f(n) ≥ c·g(n) ∀ n ≥ n₀

### 4.3 Theta Notation (Tight Bound)
f(n) = Θ(g(n)) if f(n) = O(g(n)) AND f(n) = Ω(g(n))

### 4.4 Complexity Hierarchy
```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)
```

---

## Part 5: Algorithm Design Techniques

### 5.1 Greedy Algorithm
**Approach:** Make locally optimal choice at each step

**Characteristics:**
- Iterative, top-down
- Fast, simple
- Not always optimal
- No backtracking

**Examples:**
- Dijkstra's shortest path
- Kruskal's MST
- Huffman coding
- Activity selection
- Fractional knapsack

### 5.2 Divide and Conquer
**Approach:** Divide → Conquer → Combine

**Characteristics:**
- Recursive
- Subproblems independent
- Combine results

**Recurrence:** T(n) = aT(n/b) + f(n)
**Master Theorem:**
- If f(n) = O(n^(log_b a - ε)): T(n) = Θ(n^log_b a)
- If f(n) = Θ(n^log_b a): T(n) = Θ(n^log_b a · log n)
- If f(n) = Ω(n^(log_b a + ε)): T(n) = Θ(f(n))

**Examples:**
- Merge sort
- Quick sort
- Binary search
- Strassen's matrix multiplication

### 5.3 Dynamic Programming
**Approach:** Solve subproblems, store results, reuse

**Characteristics:**
- Overlapping subproblems
- Optimal substructure
- Bottom-up or memoization
- Usually optimal solution

**Steps:**
1. Define subproblem
2. Write recurrence
3. Compute bottom-up or memoize
4. Extract solution

**Examples:**
- Fibonacci
- Longest Common Subsequence
- 0/1 Knapsack
- Matrix Chain Multiplication
- Coin Change

**DP vs Greedy:**
- DP: Exhaustive, optimal
- Greedy: Heuristic, faster, not always optimal

---

## Part 6: Graph Traversals

### 6.1 Breadth-First Search (BFS)
**Time:** O(V + E)
**Space:** O(V)
**Method:** Queue-based, level by level

**Uses:**
- Shortest path (unweighted)
- Level-order traversal
- Connected components
- Bipartiteness checking

### 6.2 Depth-First Search (DFS)
**Time:** O(V + E)
**Space:** O(h) where h = height
**Method:** Stack-based (recursive or explicit)

**Uses:**
- Topological sort
- Connected components
- Cycles detection
- Strongly connected components
- Articulation points

### 6.3 Comparison

| Property | BFS | DFS |
|----------|-----|-----|
| Data Structure | Queue | Stack |
| Time | O(V+E) | O(V+E) |
| Space | O(V) | O(h) |
| Application | Shortest path | Topological sort |
| Traversal | Level by level | Deep first |

---

## Part 7: Minimum Spanning Trees

### 7.1 Kruskal's Algorithm
**Time:** O(E log E) for sorting
**Method:** Greedy, sort edges, union-find

**Steps:**
1. Sort edges by weight
2. For each edge:
   - If connects different components: Add edge
   - Otherwise: Skip (would create cycle)

**Data Structure:** Union-Find

### 7.2 Prim's Algorithm
**Time:** O(E log V) with binary heap, O(V²) with array
**Method:** Greedy, grow tree from vertex

**Steps:**
1. Start with any vertex
2. Add minimum weight edge connecting tree to non-tree vertex
3. Repeat until all vertices included

**Data Structure:** Priority queue or array

### 7.3 Comparison

| Property | Kruskal | Prim |
|----------|---------|------|
| Approach | Edge-based | Vertex-based |
| Time | O(E log E) | O(E log V) |
| DS | Union-Find | Priority Queue |
| Sparse | Better | Worse |
| Dense | Worse | Better |

---

## Part 8: Shortest Path Algorithms

### 8.1 Dijkstra's Algorithm
**Time:** O((V + E) log V) with heap, O(V²) with array
**Precondition:** No negative weights
**Method:** Greedy, single source

**Algorithm:**
1. Initialize distances: source = 0, others = ∞
2. While unvisited vertices:
   - Select minimum distance unvisited
   - Update neighbors
3. Mark as visited

**Limitation:** Fails with negative weights

### 8.2 Bellman-Ford Algorithm
**Time:** O(VE)
**Method:** Relax all edges V-1 times
**Handles:** Negative weights (not negative cycles)

**Algorithm:**
1. Initialize distances
2. Relax all edges V-1 times
3. Check for negative cycles

**Advantage:** Handles negative weights

### 8.3 Floyd-Warshall Algorithm
**Time:** O(V³)
**Method:** All pairs shortest paths
**Formula:** dp[i][j][k] = min(dp[i][j][k-1], dp[i][k][k-1] + dp[k][j][k-1])

**Use:** All-pair distances, detects negative cycles

### 8.4 Comparison

| Algorithm | Time | Negative | Use |
|-----------|------|----------|-----|
| Dijkstra | O((V+E) log V) | No | Single source |
| Bellman-Ford | O(VE) | Yes | Single source |
| Floyd-Warshall | O(V³) | Yes | All pairs |

---

## High-Weightage Topics for GATE

1. **Sorting Algorithms** (15%)
2. **Complexity Analysis** (12%)
3. **Graph Algorithms** (18%)
4. **Dynamic Programming** (15%)
5. **Greedy Algorithms** (12%)
6. **Shortest Paths** (12%)
7. **MST** (10%)
8. **Hashing** (6%)

**Focus:** Sorting complexities, graph algorithms, DP
