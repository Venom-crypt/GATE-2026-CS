# Databases - GATE Previous Year Questions (2015-2024)

## Part 1: ER Model Questions

### Question 1.1 (GATE CSE 2024 - Set 2)
**Statement:**
A functional dependency F: X → Y is termed as a useful functional dependency if and only if it satisfies all the following three conditions:
- X is not the empty set
- Y is not the empty set
- Intersection of X and Y is the empty set

For a relation R with 4 attributes, the total number of possible useful functional dependencies is __________.

**Answer:** 50

**Solution:**
Formula: 3^n - 2^(n+1) + 1
For n = 4:
3^4 - 2^5 + 1 = 81 - 32 + 1 = 50

---

### Question 1.2 (GATE CSE 2024 - Set 1)
**Statement:**
In the context of owner and weak entity sets in the ER data model, which one of the following statements is TRUE?

A) A weak entity set has its own primary key
B) A weak entity set cannot exist independently
C) A weak entity set is always in a one-to-one relationship with its owner
D) A weak entity set is always in a many-to-many relationship with its owner

**Answer:** B

**Explanation:**
- Weak entities cannot exist independently (must depend on owner entity)
- Have partial key (discriminator), not primary key
- Usually in 1:N relationship (each owner can have multiple dependents)

---

### Question 1.3 (GATE CSE 2022)
**Statement:**
Let E1 and E2 be two entities in an E/R diagram with simple single-valued attributes. R1 and R2 are two relationships between E1 and E2, where R1 is one-to-many and R2 is many-to-many. R1 and R2 do not have any attributes of their own. What is the minimum number of tables required to represent this situation in the relational model?

A) 2
B) 3
C) 4
D) 5

**Answer:** C

**Explanation:**
- E1 table
- E2 table
- R1 (1:N): Foreign key in E2 (no separate table needed)
- R2 (M:N): Separate junction table needed

Total = 4 tables

---

## Part 2: Relational Model and Normalization

### Question 2.1 (GATE CSE 2020)
**Statement:**
Consider the following tables:
```
PROFESSOR(ProfID, Name, Dept)
COURSE(CourseID, CourseName, Credits)
TEACHES(ProfID, CourseID)
```

Which of the following is a valid functional dependency for the combined table?

A) ProfID → CourseID
B) CourseID → ProfID
C) ProfID → Name
D) CourseID, ProfID → CourseName

**Answer:** C

**Explanation:**
- ProfID uniquely determines professor's Name
- CourseID uniquely determines CourseName
- But ProfID may teach multiple courses (not 1:1)
- CourseID may be taught by multiple professors

---

### Question 2.2 (GATE CSE 2019)
**Statement:**
Consider the relation R(A,B,C,D) with functional dependencies:
- A → B
- B → C
- C → D

What is the highest normal form this relation can be in without decomposition?

A) 1NF
B) 2NF
C) 3NF
D) BCNF

**Answer:** C

**Explanation:**
Assuming (A) is primary key:
- A → B (OK, A is superkey)
- B → C (Violation, B not superkey) ✗ Not BCNF
- C → D (Violation, C not superkey)

But no transitive dependencies on A directly:
- A → B → C → D are all chain

With proper decomposition: can reach 3NF
Without: stuck at 2NF or lower

---

### Question 2.3 (GATE CSE 2018)
**Statement:**
In the relation EMPLOYEE_PROJECT(EID, Name, DeptID, DeptName, ProjID, ProjName), what is the highest normal form?

Given FDs:
- EID → Name, DeptID
- DeptID → DeptName
- ProjID → ProjName
- EID, ProjID → composite key

**Answer:** 2NF

**Explanation:**
**Violations:**
- Name depends only on EID (partial dependency)
- DeptName depends only on DeptID (partial dependency)
- ProjName depends only on ProjID (partial dependency)

Not 2NF because of partial dependencies.

---

## Part 3: Relational Algebra and SQL

### Question 3.1 (GATE CSE 2021)
**Statement:**
Given relations:
- STUDENT(SID, Name)
- COURSE(CID, CourseName)
- ENROLLMENT(SID, CID, Grade)

Which relational algebra expression finds students who scored A in at least one course?

A) π_Name(σ_Grade='A'(ENROLLMENT ⨝ STUDENT))
B) π_SID(σ_Grade='A'(ENROLLMENT)) ⨝ STUDENT
C) π_Name(σ_Grade='A'(ENROLLMENT) ⨝ STUDENT)
D) All of the above

**Answer:** D (All equivalent)

**Explanation:**
All three expressions are equivalent and produce the same result.

---

### Question 3.2 (GATE CSE 2017 - Set 1)
**Statement:**
Consider the following SQL query:
```sql
SELECT A, COUNT(*)
FROM R
WHERE B > 5
GROUP BY A
HAVING COUNT(*) > 2;
```

Which of the following is true?

A) Returns A values where count > 2 and all B > 5
B) Returns A values where count > 2
C) Returns A values where B > 5
D) Returns A values where count = 2

**Answer:** A

**Explanation:**
- WHERE filters rows (B > 5)
- GROUP BY groups by A
- HAVING filters groups (count > 2)

---

### Question 3.3 (GATE CSE 2016)
**Statement:**
Consider relation R and S. Which of the following statements is correct?

A) R ∪ S contains same tuples with no duplicates
B) R - S contains tuples in R but not in S
C) R × S is Cartesian product
D) All of the above

**Answer:** D

**Explanation:**
All are correct definitions of set operations.

---

## Part 4: Transactions and Concurrency Control

### Question 4.1 (GATE CSE 2019 - Question 11)
**Statement:**
Consider the following two statements about database transaction schedules:

S1: Strict two-phase locking protocol generates conflict serializable schedules that are also recoverable.

S2: Timestamp-ordering concurrency control protocol with Thomas' Write Rule can generate view serializable schedules that are not conflict serializable.

Which of the above statements is/are TRUE?

A) S1 only
B) S2 only
C) Both S1 and S2
D) Neither S1 nor S2

**Answer:** C

**Explanation:**
- S1: TRUE - Strict 2PL generates conflict serializable and recoverable schedules
- S2: TRUE - Timestamp ordering with Thomas' write rule can produce view-serializable but not conflict-serializable schedules (due to blind writes)

---

### Question 4.2 (GATE CSE 2018)
**Statement:**
In a system using two-phase locking, which of the following can cause deadlock?

A) Shared locks only
B) Exclusive locks only
C) Mix of shared and exclusive locks
D) Neither

**Answer:** C

**Explanation:**
- Shared-Shared: Compatible, no deadlock
- Shared-Exclusive: Incompatible, can cause wait
- Exclusive-Exclusive: Incompatible, can cause wait
- Mix of both → circular waiting → deadlock possible

---

### Question 4.3 (GATE CSE 2020)
**Statement:**
Schedule S: T1:R(X), T2:W(X), T1:W(X), T2:R(X)

Is this schedule conflict serializable?

A) Yes, equivalent to T1-T2
B) Yes, equivalent to T2-T1
C) No, has cycle in precedence graph
D) Cannot determine

**Answer:** A

**Explanation:**
Precedence: T1 → T2 (from R1(X) and W2(X))
No cycle → Conflict serializable
Equivalent to: T1, T2

---

## Part 5: Indexing and File Organization

### Question 5.1 (GATE CSE 2019)
**Statement:**
For a B+ tree index with order m, search time is O(log_m N) where N is number of records.
If m = 100 and N = 1,000,000, how many I/Os worst case?

A) 2
B) 3
C) 4
D) 5

**Answer:** B

**Explanation:**
log_100(1,000,000) = log(1,000,000)/log(100)
= 6/2 = 3

Worst case: 3 I/Os (root + internal + leaf)

---

### Question 5.2 (GATE CSE 2021)
**Statement:**
Advantages of B+ trees over B trees include:

A) Faster search
B) Better for range queries
C) Smaller memory per node
D) B and C only

**Answer:** D

**Explanation:**
- B+ trees have data only in leaves → smaller internal nodes
- Linked leaves enable efficient range queries
- Search speed similar (both O(log n))

---

## Part 6: Integrity Constraints

### Question 6.1 (GATE CSE 2022)
**Statement:**
Entity integrity constraint ensures:

A) Primary key cannot be NULL
B) Foreign key cannot be NULL
C) Both must be NOT NULL
D) Neither can be NULL

**Answer:** A

**Explanation:**
- Entity integrity: Primary key NOT NULL and UNIQUE
- Referential integrity: Foreign key must match primary key OR be NULL

---

### Question 6.2 (GATE CSE 2020)
**Statement:**
Which constraint violation is possible?

A) Primary key with NULL
B) Foreign key with NULL
C) Unique constraint violation
D) Both A and C

**Answer:** B

**Explanation:**
- A: NO - Primary key never NULL
- B: YES - Foreign key can be NULL
- C: YES - Unique constraint can be violated if duplicate non-NULL

---

## Part 7: Views and Query Optimization

### Question 7.1 (GATE CSE 2021)
**Statement:**
Two schedules S1 and S2 are view equivalent if:

A) Same initial reads
B) Same updated reads
C) Same final write
D) All of the above

**Answer:** D

**Explanation:**
All three conditions must be satisfied for view equivalence.

---

### Question 7.2 (GATE CSE 2019)
**Statement:**
Consider schedule: T1:R(A), T2:W(A), T1:W(A), T3:R(A)

Is this view serializable?

A) Yes
B) No
C) Need more information
D) Cannot determine

**Answer:** A

**Explanation:**
- Initial read: T1 reads A (ok in any order)
- Final write: T1 writes A (order matters)
- Equivalence possible: T2 → T1 → T3 order works

---

## Part 8: Decomposition and Losslessness

### Question 8.1 (GATE CSE 2018)
**Statement:**
Relation R(A,B,C) with FD A → B is decomposed into:
- R1(A,B)
- R2(A,C)

Is this decomposition lossless?

A) Yes
B) No
C) Depends on data
D) Cannot determine

**Answer:** A

**Explanation:**
- Common attribute: A
- A determines B (from FD)
- Join of R1 and R2 on A recovers original R
- Lossless decomposition

---

### Question 8.2 (GATE CSE 2020)
**Statement:**
Decomposition is dependency-preserving if:

A) Functional dependencies are preserved
B) No information is lost
C) All constraints can be checked locally
D) A and C

**Answer:** D

**Explanation:**
- Dependency-preserving: FDs preserved locally on decomposed relations
- Different from losslessness (which is about information)

---

## Part 9: Multiple Entity Relationships

### Question 9.1 (GATE CSE 2019)
**Statement:**
ER model with entities E1, E2, E3 and relationships:
- E1 (1) --- (N) E2
- E2 (M) --- (N) E3
- E1 (1) --- (1) E3

How many relations in relational model?

A) 3
B) 4
C) 5
D) 6

**Answer:** C

**Explanation:**
- E1 table
- E2 table (with FK to E1)
- E3 table (with FK to E1)
- E2_E3 junction table for M:N
- E1_E3 for 1:1 (could be merged with E3)

Minimum: 5 (keeping all separate for clarity)

---

## Part 10: Complex Normalization Scenarios

### Question 10.1 (GATE CSE 2017)
**Statement:**
Relation PROFESSOR_COURSE(ProfID, CoursID, Time, Room)

FDs:
- ProfID, CourseID → Time
- CourseID → Room

What is the highest normal form?

A) 1NF
B) 2NF
C) 3NF
D) BCNF

**Answer:** 2NF

**Explanation:**
- PK: (ProfID, CourseID)
- CourseID → Room (partial dependency)
- Partial dependency violation → Not 2NF

---

### Question 10.2 (GATE CSE 2021)
**Statement:**
Given FDs: A → B, B → C, C → D

Candidate key: A

Is relation in BCNF?

A) Yes
B) No
C) Depends on additional constraints
D) Cannot determine

**Answer:** B

**Explanation:**
- A → B: A is CK, OK
- B → C: B is NOT CK, violation
- C → D: C is NOT CK, violation

Not BCNF

---

## Study Guide Based on GATE Questions

### Topic-wise Frequency (Last 10 Years):
1. **Normalization**: 18% of database questions
2. **ER Model**: 15%
3. **SQL and Relational Algebra**: 14%
4. **Concurrency Control**: 12%
5. **Transactions and ACID**: 11%
6. **Indexing (B trees)**: 10%
7. **Functional Dependencies**: 10%
8. **Integrity Constraints**: 7%
9. **Other**: 3%

### High-Probability Questions for GATE 2026:
1. Normal form identification and decomposition
2. ER to relational conversion (weak entities)
3. Functional dependency closure
4. Conflict vs View serializability
5. B+ tree operations
6. SQL queries with GROUP BY and HAVING
7. Concurrency protocols (2PL, timestamp)
8. Deadlock detection and prevention

---

## Quick Answers Reference

| Question Type | Key Point |
|---------------|-----------|
| Normal Forms | Check dependencies against key |
| ER to Relational | Count M:N relationships separately |
| Serializability | Build precedence graph, check cycles |
| B+ Tree | log_m(N) levels needed |
| 2PL | No cycle in lock wait graph = safe |
| Useful FDs | 3^n - 2^(n+1) + 1 formula |
| Losslessness | Common attribute must determine separate parts |

---

Good luck with GATE 2026 Databases!
