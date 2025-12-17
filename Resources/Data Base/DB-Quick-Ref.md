# Databases - GATE 2026 Quick Reference Sheet

## ER Model Quick Reference

### ER Components
| Component | Symbol | Purpose |
|-----------|--------|---------|
| Entity | Rectangle | Real-world object |
| Attribute | Oval | Property of entity |
| Relationship | Diamond | Association between entities |
| Primary Key | Underlined | Unique identifier |
| Multi-valued | Double Oval | Multiple values |
| Derived | Dashed Oval | Calculated attribute |
| Weak Entity | Double Rectangle | Depends on owner |
| Identifying Rel | Double Diamond | Defines weak entity |

### Cardinality Notations
- **1:1** - One-to-One (one entity related to one)
- **1:N** - One-to-Many (one related to many)
- **M:N** - Many-to-Many (many related to many)

### Participation
- **Total** (Double line) - Every entity participates
- **Partial** (Single line) - Some entities may not participate

---

## Relational Model Quick Reference

### Components
| Term | Definition |
|------|-----------|
| Relation | Table with rows and columns |
| Tuple | Row/Record |
| Attribute | Column/Field |
| Degree | Number of attributes |
| Cardinality | Number of tuples |
| Domain | Set of allowed values |
| Schema | Structure definition |
| Instance | Actual data at given time |

### Keys
| Key Type | Definition |
|----------|-----------|
| Candidate Key | Minimal set uniquely identifying tuple |
| Primary Key | Chosen candidate key (NOT NULL, UNIQUE) |
| Super Key | Set of attributes identifying tuple (can have extras) |
| Foreign Key | References primary key in another relation |
| Partial Key | Discriminator for weak entity |

---

## Relational Algebra Operations

### Unary Operations
| Operation | Symbol | Purpose | SQL Equivalent |
|-----------|--------|---------|----------------|
| Selection | σ_(condition) | Filter rows | WHERE |
| Projection | π_(attributes) | Select columns | SELECT |
| Rename | ρ | Rename relation/attributes | AS |

### Binary Operations
| Operation | Symbol | Purpose |
|-----------|--------|---------|
| Union | ∪ | All tuples from both (distinct) |
| Intersection | ∩ | Common tuples |
| Set Difference | − | In first but not second |
| Cartesian Product | × | All combinations |

### Derived Operations
| Operation | Purpose |
|-----------|---------|
| Natural Join ⨝ | Join on common attributes |
| Theta Join ⨝_(condition) | Join with any condition |
| Division ÷ | Find X where for all Y condition holds |
| Aggregate | GROUP BY with COUNT, SUM, AVG, MIN, MAX |

---

## SQL Quick Reference

### Basic Syntax
```sql
SELECT [DISTINCT] columns
FROM table
WHERE condition
GROUP BY columns
HAVING group_condition
ORDER BY column [ASC|DESC];
```

### WHERE Operators
- Comparison: =, <>, <, >, <=, >=
- Logical: AND, OR, NOT
- Pattern: LIKE (%, _), IN, BETWEEN
- NULL: IS NULL, IS NOT NULL

### JOIN Types
```sql
INNER JOIN - Common rows
LEFT OUTER JOIN - All from left + matching from right
RIGHT OUTER JOIN - All from right + matching from left
FULL OUTER JOIN - All from both
NATURAL JOIN - Join on common attributes
CROSS JOIN - Cartesian product
```

### Aggregate Functions
- COUNT(*) - Number of rows
- SUM(column) - Total
- AVG(column) - Average
- MIN(column) - Minimum
- MAX(column) - Maximum

### GROUP BY Rules
- SELECT clause can have:
  - Grouped columns
  - Aggregate functions
  - Constants
- Non-grouped columns not allowed

---

## Tuple Relational Calculus

### Syntax
```
{t | P(t)}
t = tuple variable
P(t) = predicate/condition
```

### Quantifiers
- **∃ (Exists)**: "There exists at least one"
- **∀ (For all)**: "For all"

### Logical Operators
- **∧ (AND)** - Both conditions true
- **∨ (OR)** - At least one true
- **¬ (NOT)** - Negation

---

## Functional Dependencies

### Definition
X → Y (X determines Y): If two tuples have same X value, they have same Y value

### Types
| Type | Definition |
|------|-----------|
| Trivial | Y ⊆ X (always true) |
| Non-trivial | At least one attr in Y not in X |
| Useful | X ≠ ∅, Y ≠ ∅, X ∩ Y = ∅ |
| Partial | Non-key depends on subset of composite key |
| Transitive | A → B and B → C (implies A → C) |

### Armstrong's Axioms
1. **Reflexivity**: If Y ⊆ X, then X → Y
2. **Augmentation**: If X → Y, then XZ → YZ
3. **Transitivity**: If X → Y and Y → Z, then X → Z

### Closure of X (X+)
Set of all attributes determined by X

---

## Normal Forms

### 1NF (First Normal Form)
- **Condition**: No multi-valued attributes
- **Check**: Each cell contains atomic value
- **Fix**: Separate multi-valued into new relation

### 2NF (Second Normal Form)
- **Condition**: In 1NF + No partial dependencies
- **Check**: Non-key attributes fully depend on entire primary key
- **Fix**: Separate partial dependencies into new relations

### 3NF (Third Normal Form)
- **Condition**: In 2NF + No transitive dependencies
- **Check**: Non-key attributes directly depend on primary key
- **Fix**: Separate transitive dependencies

### BCNF (Boyce-Codd Normal Form)
- **Condition**: Every determinant is candidate key
- **Check**: For every A → B, A is superkey
- **Note**: Most strict, strongest guarantee

### Normalization Decision Tree
```
Relation has multi-valued attributes?
↓
YES → Separate → 1NF
NO → 1NF

Non-key depends on partial composite key?
↓
YES → Separate → 2NF
NO → 2NF

Non-key depends on another non-key?
↓
YES → Separate → 3NF
NO → 3NF

Determinant not candidate key?
↓
YES → Separate → BCNF
NO → BCNF
```

---

## File Organization

### Heap File
- **Insertion**: O(1)
- **Deletion**: O(n)
- **Search**: O(n)
- **Best for**: Few searches, many insertions

### Sequential (Sorted)
- **Insertion**: O(n)
- **Deletion**: O(log n + n)
- **Search**: O(log n)
- **Best for**: Many searches, sorted access

### B-Tree
- **All operations**: O(log n)
- **Data pointers**: In every node
- **Unique keys**: No redundancy
- **Best for**: Random access

### B+ Tree
- **All operations**: O(log n)
- **Data pointers**: Leaf nodes only
- **Redundant keys**: In internal nodes
- **Linked leaves**: Sequential access
- **Best for**: Range queries, databases

### B vs B+ Comparison

| Feature | B-Tree | B+ Tree |
|---------|--------|---------|
| Data location | Every node | Leaf only |
| Range queries | Less efficient | Efficient |
| Redundant keys | No | Yes |
| Sequential scan | No | Yes |
| Memory per node | More | Less |

---

## Transactions and ACID

### ACID Properties

| Property | Meaning | Ensures |
|----------|---------|---------|
| **A**tomicity | All or nothing | Transaction completes fully or not at all |
| **C**onsistency | Valid to valid | Integrity constraints maintained |
| **I**solation | Independent | No interference between transactions |
| **D**urability | Permanent | Committed changes survive failures |

### ACID Implementation
- **Atomicity**: Logging (WAL - Write-Ahead Logging)
- **Consistency**: Constraint checking
- **Isolation**: Locking or timestamp ordering
- **Durability**: Stable storage, backups

---

## Concurrency Control Problems

| Problem | Description | Solution |
|---------|-------------|----------|
| **Dirty Read** | Read uncommitted data | 2PL, Isolation levels |
| **Lost Update** | Update overwritten | Locking |
| **Non-repeatable Read** | Different reads in transaction | Locking |
| **Phantom Read** | New rows appear mid-transaction | Isolation levels |

---

## Lock-Based Concurrency Control

### Lock Types
- **Shared Lock (S)**: Multiple readers
- **Exclusive Lock (X)**: Single writer

### Lock Compatibility Matrix
```
        | S | X |
--------|---|---|
Hold S  | Y | N |
Hold X  | N | N |
```

### Two-Phase Locking (2PL)
1. **Growing Phase**: Acquire locks, no releases
2. **Shrinking Phase**: Release locks, no acquisitions

**Guarantee**: Conflict serializable schedules

### Variants
- **Conservative 2PL**: All locks initially
- **Strict 2PL**: Hold exclusive locks until commit
- **Rigorous 2PL**: Hold all locks until commit

---

## Deadlock Prevention

### Wait-Die (Non-preemptive)
```
If T_new requests lock held by T_old:
  T_new older than T_old: Wait
  T_new younger than T_old: Die (abort)
```

### Wound-Wait (Preemptive)
```
If T_new requests lock held by T_old:
  T_new older than T_old: T_old is wounded (abort)
  T_new younger than T_old: Wait
```

---

## Serializability

### Conflict Serializability
- Operations conflict if different transactions, same data, ≥1 write
- Schedule serializable if transformable to serial by swapping non-conflicts
- **Check**: Precedence graph - if no cycles, serializable

### View Serializability
- More general than conflict serializability
- **Conditions**:
  1. Same initial reads
  2. Same updated reads
  3. Same final writes
- Handles blind writes (write without read)

### Serializability Hierarchy
```
Conflict Serializable ⊂ View Serializable ⊂ All Schedules
(More restricted)                (More general)
```

---

## Timestamp Ordering

### Basic Approach
- Assign unique timestamp to each transaction
- Each data item has R-TS(X) and W-TS(X)

**Rules:**
- If TS(Ti) < R-TS(X): Abort Ti
- If TS(Ti) < W-TS(X): Abort Ti

### Thomas' Write Rule
- Modification: Ignore write if TS(Ti) < W-TS(X)
- Improves concurrency

---

## Important Formulas

### Closure of X
```
X+ = all attributes determined by X
Algorithm: Start with X, repeatedly apply FDs
```

### Number of Useful FDs
```
For n attributes:
3^n - 2^(n+1) + 1 (approximately)
```

### B-Tree Order Calculation
```
Order m: each node max m children, m-1 keys
Max keys in m-ary tree with h levels: m^h - 1
```

### Conflict Operations
```
Two operations conflict if:
1. Different transactions
2. Same data item
3. At least one is write
```

---

## Common GATE Exam Patterns

### High-Frequency Topics
1. **Normalization**: 20% of questions
   - Identify violations
   - Decompose relations
   - Determine normal form

2. **ER Conversion**: 15%
   - ER to relational tables
   - Cardinality handling
   - Weak entities

3. **SQL Queries**: 12%
   - Complex joins
   - GROUP BY with HAVING
   - Subqueries

4. **Concurrency**: 10%
   - Serializability checking
   - Lock protocols
   - Deadlock detection

5. **Relational Algebra**: 12%
   - Query optimization
   - Equivalent expressions
   - Operation ordering

### Exam Tricks
- Watch multi-valued attributes
- Check for transitive dependencies
- Identify weak entities carefully
- SQL output depends on NULLs
- Precedence graph cycles = not serializable

---

## Quick Problem-Solving Checklist

**For Normalization:**
- [ ] Check for multi-valued attributes (1NF)
- [ ] Check partial dependencies (2NF)
- [ ] Check transitive dependencies (3NF)
- [ ] Check determinants are superkeys (BCNF)

**For ER Conversion:**
- [ ] Strong entities → relations
- [ ] Attributes → columns
- [ ] 1:1 relationships → combine or foreign keys
- [ ] 1:N relationships → foreign key in N-side
- [ ] M:N relationships → new relation with foreign keys
- [ ] Weak entities → including owner's primary key

**For SQL Queries:**
- [ ] Check NULL handling
- [ ] Verify GROUP BY includes all non-aggregate columns
- [ ] HAVING filters after GROUP BY
- [ ] JOIN conditions correct
- [ ] Aggregate functions on grouped data

**For Serializability:**
- [ ] Build precedence graph
- [ ] Check for cycles
- [ ] If no cycle = Conflict Serializable

---

Print this sheet for last-minute revision!
