# Algorithms - GATE 2026 Quick Reference

## Sorting Algorithms Cheat Sheet

| Algorithm | Best | Average | Worst | Space | Stable | Method |
|-----------|------|---------|-------|-------|--------|--------|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | ✓ | Compare adjacent |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | ✗ | Find min |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | ✓ | Shift elements |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | ✓ | Divide-merge |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | ✗ | Partition |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | ✗ | Heap operations |
| Counting | O(n+k) | O(n+k) | O(n+k) | O(n+k) | ✓ | Count elements |
| Radix | O(nk) | O(nk) | O(nk) | O(n+k) | ✓ | Digit sorting |

## Searching Algorithms

**Linear Search:** O(n) best/avg/worst, O(1) space
**Binary Search:** O(log n) best/avg/worst, O(1) space (requires sorted)

## Asymptotic Complexity

### Hierarchy
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)

### Big O, Omega, Theta
- **Big O:** Upper bound (worst case)
- **Omega (Ω):** Lower bound (best case)
- **Theta (Θ):** Tight bound (average case)

### Master Theorem
T(n) = aT(n/b) + f(n)

If f(n) = O(n^log_b a - ε): T(n) = Θ(n^log_b a)
If f(n) = Θ(n^log_b a): T(n) = Θ(n^log_b a · log n)
If f(n) = Ω(n^log_b a + ε): T(n) = Θ(f(n))

## Hashing

**Hash Function:** h(k) = k mod m

**Collision Resolution:**
1. **Chaining:** Linked lists, O(1+α) average
2. **Linear Probing:** h(k,i) = (h(k)+i) mod m, clustering
3. **Quadratic Probing:** h(k,i) = (h(k)+c₁i+c₂i²) mod m
4. **Double Hashing:** h(k,i) = (h₁(k)+i·h₂(k)) mod m

**Load Factor:** α = n/m (keep < 0.7 for open addressing)

## Algorithm Design Techniques

### Greedy
- **Approach:** Locally optimal at each step
- **Time:** Usually polynomial
- **Optimal:** Not always
- **Examples:** Dijkstra, Kruskal, activity selection
- **Use:** When greedy choice always leads to optimal

### Divide and Conquer
- **Approach:** Divide → Solve → Combine
- **Subproblems:** Independent
- **Time:** Master theorem
- **Examples:** Merge sort, quick sort, binary search
- **Use:** When independent subproblems

### Dynamic Programming
- **Approach:** Overlapping subproblems + memoization
- **Time:** Usually polynomial, depends on states
- **Optimal:** Usually yes
- **Examples:** Fibonacci, LCS, knapsack, DP distance
- **Use:** Overlapping subproblems with optimal substructure

**DP recurrence types:**
- **0/1 Knapsack:** dp[i][w] = max(dp[i-1][w], dp[i-1][w-weight[i]] + value[i])
- **LCS:** dp[i][j] = dp[i-1][j-1]+1 if s1[i]==s2[j], else max(dp[i-1][j], dp[i][j-1])
- **Coin Change:** dp[i] = min(dp[i-coin] + 1) for all coins

## Graph Algorithms

### BFS (Breadth-First Search)
- **Time:** O(V + E)
- **Space:** O(V)
- **Use:** Shortest path (unweighted), level-order
- **Data:** Queue

### DFS (Depth-First Search)
- **Time:** O(V + E)
- **Space:** O(h) height
- **Use:** Topological sort, cycles, SCC
- **Data:** Stack

### Minimum Spanning Trees

**Kruskal's Algorithm:**
- **Time:** O(E log E) sorting
- **Method:** Sort edges, union-find
- **Best for:** Sparse graphs

**Prim's Algorithm:**
- **Time:** O(E log V) with heap
- **Method:** Grow tree from vertex
- **Best for:** Dense graphs

### Shortest Path Algorithms

**Dijkstra:**
- **Time:** O((V+E) log V) heap, O(V²) array
- **Precondition:** Non-negative weights
- **Single source** shortest path
- **Greedy** approach

**Bellman-Ford:**
- **Time:** O(VE)
- **Handles:** Negative weights
- **Detects:** Negative cycles
- **Single source** shortest path

**Floyd-Warshall:**
- **Time:** O(V³)
- **All pairs** shortest paths
- **Handles:** Negative weights
- **Detects:** Negative cycles
- **Formula:** dp[i][j][k] = min(dp[i][j][k-1], dp[i][k][k-1] + dp[k][j][k-1])

## Recurrence Relations

**T(n) = T(n-1) + O(n):** O(n²) [insertion sort]
**T(n) = 2T(n/2) + O(n):** O(n log n) [merge sort]
**T(n) = T(n/2) + O(1):** O(log n) [binary search]
**T(n) = nT(n-1):** O(n!) [permutations]

## Stability in Sorting

**Stable:** Bubble, Insertion, Merge, Counting, Radix
**Unstable:** Selection, Quick, Heap

## Problem Pattern Recognition

**Pattern 1:** Find minimum/maximum
→ Greedy or DP

**Pattern 2:** Count ways/paths
→ DP (combinatorics)

**Pattern 3:** Shortest/longest
→ DP or graph algorithm

**Pattern 4:** Optimization on set
→ Greedy or DP

**Pattern 5:** Search/find in array
→ Binary search or hashing

**Pattern 6:** Graph connectivity
→ BFS/DFS

**Pattern 7:** Tree minimum weight
→ MST algorithm

## Complexity Quick Facts

- **Sorting lower bound:** Ω(n log n) comparison-based
- **Comparison operations:** O(n²) quadratic sorts
- **Hashing average:** O(1) insert/delete/search with uniform distribution
- **Graph traversal:** O(V+E) both BFS and DFS
- **MST algorithms:** O(E log V) efficient
- **All-pairs shortest:** O(V³) Floyd-Warshall

## GATE Question Patterns

**Type 1: Complexity Analysis**
- Identify Big O, Omega, Theta
- Master theorem application
- Recurrence solving

**Type 2: Sorting**
- Which algorithm for scenario?
- Stability, space, time trade-offs
- Sorting specific cases

**Type 3: Dynamic Programming**
- Design recurrence
- Count table size
- Optimize space/time

**Type 4: Graph Algorithms**
- Apply Dijkstra, Kruskal, etc.
- MST weight, shortest path
- Graph properties (connected, cyclic, etc.)

**Type 5: Greedy Justification**
- When is greedy optimal?
- Counterexamples for non-optimal

## Common Mistakes

✗ Using Dijkstra with negative weights
✗ Confusing Kruskal (edge-based) and Prim (vertex-based)
✗ Not considering space complexity
✗ Wrong recurrence setup for DP
✗ Incorrect master theorem application
✗ Forgetting stability requirement in sorting
✗ Using Big O when Theta is meant (loose vs tight bound)

---

**Print for exam day revision!**
