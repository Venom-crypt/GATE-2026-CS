# Databases - GATE 2026 Practice Problems

## Part 1: ER Model and Conversion

### Problem 1.1
Draw ER diagram for University database with:
- Student: StudentID, Name, Email
- Course: CourseID, CourseName, Credits
- Department: DepartmentID, DeptName
- Relationships: Student belongs to Department (1:N), Student enrolls in Course (M:N)

Convert to relational model.

**Solution:**
**ER Diagram:**
```
DEPARTMENT (1) --- (N) STUDENT
STUDENT (M) --- (N) COURSE
```

**Relational Schema:**
```
DEPARTMENT(DepartmentID, DeptName)
STUDENT(StudentID, Name, Email, DepartmentID)
  Foreign Key: DepartmentID → DEPARTMENT
COURSE(CourseID, CourseName, Credits)
ENROLLMENT(StudentID, CourseID)
  Foreign Keys: StudentID → STUDENT, CourseID → COURSE
  Primary Key: (StudentID, CourseID)
```

### Problem 1.2
Consider ER diagram with:
- Employee (1) --- (1) Passport (weak entity with partial key PassportNo)
- Employee (1) --- (M) Dependent (weak entity)

How many tables needed?

**Solution:**
```
EMPLOYEE table with EmpID
PASSPORT table with (EmpID, PassportNo) - composite PK
DEPENDENT table with (EmpID, DependentNo) - composite PK

Total: 3 tables
```

### Problem 1.3
ER diagram shows:
- E1 and E2 connected by R1 (1:N) relationship
- E1 and E2 connected by R2 (M:N) relationship
- Both relationships have no attributes

Minimum tables required?

**Solution:**
```
E1 table
E2 table
R1 relationship → Foreign key in E2
R2 relationship → Separate junction table

Total: 4 tables (E1, E2, R1_junction, R2_junction)
```

---

## Part 2: Normalization Problems

### Problem 2.1
Analyze relation STUDENT_COURSE(StudentID, Name, Email, CourseID, CourseName, Grade)

Identify normal form violations and decompose.

**Solution:**
**Violations:**
- Partial dependency: Name, Email depend only on StudentID (not on CourseID)
- Causes: Not in 2NF

**Decomposition:**
```
STUDENT(StudentID, Name, Email)
  PK: StudentID
COURSE(CourseID, CourseName)
  PK: CourseID
ENROLLMENT(StudentID, CourseID, Grade)
  PK: (StudentID, CourseID)
  FKs: StudentID → STUDENT, CourseID → COURSE
```

### Problem 2.2
Relation: PROFESSOR_COURSE(ProfID, ProfName, CourseID, CourseName, Time)

Determine normal form.

**Solution:**
**Functional Dependencies:**
- ProfID → ProfName
- CourseID → CourseName, Time

**Analysis:**
- Not in 3NF: ProfName depends on ProfID (non-key)
- Not in 3NF: CourseName, Time depend on CourseID
- Not in BCNF: CourseID → Time, but CourseID not candidate key

**Violations:**
```
Transitive dependencies:
- (ProfID, CourseID) → CourseID → CourseName, Time
```

**Decomposition:**
```
PROFESSOR(ProfID, ProfName)
COURSE(CourseID, CourseName, Time)
TEACHES(ProfID, CourseID)
```

### Problem 2.3
Relation: R(A, B, C, D) with FDs: A → B, C → D

Is it in 3NF? If not, decompose.

**Solution:**
**Keys:** (A, C) is candidate key (can derive all attributes)

**Check 3NF:**
- A → B: A is super key ✓
- C → D: C is NOT super key ✗

**Not in 3NF**

**Decomposition:**
```
R1(A, B)
R2(C, D)
R3(A, C)
  PK: (A, C)
```

### Problem 2.4
Useful functional dependencies count

For relation with 4 attributes {A, B, C, D}, how many useful FDs?

**Solution:**
Useful FD: X → Y where X ≠ ∅, Y ≠ ∅, X ∩ Y = ∅

Formula: 3^n - 2^(n+1) + 1

For n = 4:
3^4 - 2^5 + 1 = 81 - 32 + 1 = **50 useful FDs**

---

## Part 3: Relational Algebra

### Problem 3.1
Tables:
- STUDENT(SID, Name, Dept)
- COURSE(CID, CourseName, DeptID)
- ENROLLMENT(SID, CID, Grade)

Find: Names of students in CS department who have grade A

**Solution:**
```
π_Name(
  σ_(Dept='CS' ∧ Grade='A')(
    STUDENT ⨝ ENROLLMENT ⨝ COURSE
  )
)
```

Or stepwise:
```
CS_Students = σ_(Dept='CS')(STUDENT)
A_Grades = σ_(Grade='A')(ENROLLMENT)
Result = π_Name(CS_Students ⨝ A_Grades ⨝ COURSE)
```

### Problem 3.2
Find students who have taken ALL courses

**Solution:**
```
ENROLLMENT ÷ π_CID(COURSE)

Explanation:
- Numerator: (SID, CID) pairs (who took what)
- Denominator: All course IDs
- Result: SID values where student has CID for every course
```

### Problem 3.3
Optimize query:

```
π_Name(σ_(Age>20 ∧ GPA>3.0)(STUDENT))
```

**Solution:**
Already optimized:
```
π_Name(
  σ_(Age>20)(
    σ_(GPA>3.0)(STUDENT)
  )
)
```

Execution plan:
1. Filter GPA > 3.0 (reduces tuples)
2. Filter Age > 20 (reduces further)
3. Project Name

---

## Part 4: SQL Queries

### Problem 4.1
Find average salary by department, showing only departments with avg salary > 50000

**Solution:**
```sql
SELECT Department, AVG(Salary) as AvgSal
FROM EMPLOYEE
GROUP BY Department
HAVING AVG(Salary) > 50000;
```

### Problem 4.2
Find students with name starting with 'A' enrolled in course 'CSE101'

**Solution:**
```sql
SELECT DISTINCT s.StudentID, s.Name
FROM STUDENT s
INNER JOIN ENROLLMENT e ON s.StudentID = e.StudentID
WHERE s.Name LIKE 'A%' AND e.CourseID = 'CSE101';
```

### Problem 4.3
Find courses with more than 50 students enrolled

**Solution:**
```sql
SELECT CourseID, COUNT(StudentID) as StudentCount
FROM ENROLLMENT
GROUP BY CourseID
HAVING COUNT(StudentID) > 50;
```

### Problem 4.4
Find students who have taken ALL courses offered

**Solution:**
```sql
SELECT StudentID
FROM ENROLLMENT
GROUP BY StudentID
HAVING COUNT(DISTINCT CourseID) = (SELECT COUNT(*) FROM COURSE);
```

---

## Part 5: Transactions and Concurrency

### Problem 5.1
Schedule Analysis

Transaction T1: R(A), W(A)
Transaction T2: W(A), R(A)

Schedule S: R1(A), W2(A), W1(A), R2(A)

Is it conflict serializable?

**Solution:**
**Conflicts:**
- R1(A) and W2(A): T1 → T2
- W1(A) and R2(A): T1 → T2
- W1(A) and W2(A): conflict but already ordered

**Precedence Graph:** T1 → T2 (no cycle)

**Answer: YES, Conflict Serializable**

Equivalent serial: T1, T2

### Problem 5.2
Apply 2PL locking protocol

T1: Read(A), Write(A), Read(B), Write(B)
T2: Read(B), Write(B)

**Solution:**
**T1 with 2PL:**
```
Growing Phase:
  Lock-S(A)
  Read(A)
  Lock-X(A)
  Write(A)
  Lock-S(B)
  Read(B)
  Lock-X(B)
  Write(B)
Shrinking Phase:
  Unlock(A)
  Unlock(B) (or after lock-X(B) depending on variant)
```

**T2 with 2PL:**
```
Growing Phase:
  Lock-S(B)
  Read(B)
  Lock-X(B)
  Write(B)
Shrinking Phase:
  Unlock(B)
```

---

## Part 6: B+ Trees

### Problem 6.1
Insert values: 10, 20, 5, 6, 12, 30, 7, 17 into B+ tree of order 3

**Solution:**
```
Order 3: Max 2 keys per node, 3 children

Insertions result:
        [12]
       /    \
    [6,10]  [20,30]
    /|  |   /|   |
  [5] [7] [10] [12] [17] [20] [30]

(Leaf nodes linked for range queries)
```

### Problem 6.2
Search for 15 in B+ tree

**Solution:**
```
1. Start at root [12]
2. 15 > 12, go right
3. Reach leaf [12, 17, 20]
4. 15 not found
5. Return "Not found"

Cost: 2 disk I/O (root + leaf)
```

### Problem 6.3
B vs B+ tree comparison

For n = 1000 keys, order m = 100:
- B-tree levels needed?
- B+ tree levels needed?

**Solution:**
```
B-tree: min occupancy ⌈100/2⌉ = 50 keys per node
  Max keys in tree: 100^3 - 1 ≈ 1,000,000
  Levels: 3

B+ tree: Same structure for internal nodes
  Also 3 levels
```

---

## Part 7: Functional Dependencies

### Problem 7.1
Given FDs: A → B, B → C, A → D

Find A+

**Solution:**
```
A+ = {A}
Apply A → B: A+ = {A, B}
Apply B → C: A+ = {A, B, C}
Apply A → D: A+ = {A, B, C, D}
```

### Problem 7.2
Check if F: A → B, B → C logically implies A → C

**Solution:**
**Using Transitivity (Armstrong's axiom 3):**
- Given: A → B
- Given: B → C
- By transitivity: A → C

**Answer: YES**

### Problem 7.3
Find candidate keys

Relation R(A, B, C, D, E) with FDs:
- A → B
- C → D
- (A, C) → E

**Solution:**
```
Check (A, C)+:
- A → B: (A, C)+ = {A, C, B}
- C → D: (A, C)+ = {A, C, B, D}
- (A, C) → E: (A, C)+ = {A, C, B, D, E}

(A, C)+ = {A, C, B, D, E} = All attributes

Candidate Key: (A, C)
```

---

## Part 8: Integrity Constraints

### Problem 8.1
Implement schema with constraints:

Students can't have NULL ID or email
Department ID must exist in DEPARTMENT table
Email must be unique

**Solution:**
```sql
CREATE TABLE STUDENT (
    StudentID INT NOT NULL PRIMARY KEY,
    Name VARCHAR(50) NOT NULL,
    Email VARCHAR(100) NOT NULL UNIQUE,
    DepartmentID INT NOT NULL,
    FOREIGN KEY (DepartmentID) REFERENCES DEPARTMENT(DepartmentID)
);
```

### Problem 8.2
Referential integrity violation

Insert: (001, 'Alice', 'alice@x.com', 999)
Where DepartmentID 999 doesn't exist in DEPARTMENT

**Solution:**
**Result:** INSERT fails
**Reason:** Foreign key constraint violated
**Fix:** Insert valid DepartmentID or insert into DEPARTMENT first

---

## Part 9: Serializability Problems

### Problem 9.1
Schedule: T1:R(X), T2:W(X), T1:W(X), T2:R(X)

Check conflict serializability

**Solution:**
**Conflicts:**
- T1:R(X) vs T2:W(X): T1 → T2
- T1:W(X) vs T2:R(X): T1 → T2

**Precedence:** T1 → T2 (no cycle)

**Answer: Conflict Serializable**

### Problem 9.2
Identify dirty read vulnerability

T1: Read(A) = 100, A = A + 50
T2: Read(A) = 150
T1: Rollback

**Solution:**
**Issue:** T2 read dirty data (uncommitted update)

**Prevention:**
- Don't release shared lock until commit
- Isolation level: READ COMMITTED or higher

---

## Part 10: Deadlock and Locking

### Problem 10.1
Wait-Die vs Wound-Wait

T1 (older, TS=5) requests lock on X held by T2 (younger, TS=10)

**Solution:**
**Wait-Die:**
- T1 older than T2: T1 waits

**Wound-Wait:**
- T1 older than T2: T2 is wounded (aborts)

---

## Part 11: Mixed Problems

### Problem 11.1
Complete scenario:

Database: Bank accounts
Tables: ACCOUNT(AccID, Balance), TRANSACTION(TransID, FromAccID, ToAccID, Amount)

1. Design ER model
2. Create relational schema
3. Write SQL for transfer
4. Ensure ACID properties

**Solution:**

**1. ER Model:**
```
ACCOUNT --- (1:N) --- TRANSACTION (From)
ACCOUNT --- (1:N) --- TRANSACTION (To)
```

**2. Relational Schema:**
```sql
CREATE TABLE ACCOUNT (
    AccID INT PRIMARY KEY,
    Balance DECIMAL(10,2) NOT NULL
);

CREATE TABLE TRANSACTION (
    TransID INT PRIMARY KEY,
    FromAccID INT NOT NULL,
    ToAccID INT NOT NULL,
    Amount DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (FromAccID) REFERENCES ACCOUNT(AccID),
    FOREIGN KEY (ToAccID) REFERENCES ACCOUNT(AccID)
);
```

**3. Transfer SQL:**
```sql
BEGIN TRANSACTION;
UPDATE ACCOUNT SET Balance = Balance - 100 WHERE AccID = 1;
UPDATE ACCOUNT SET Balance = Balance + 100 WHERE AccID = 2;
INSERT INTO TRANSACTION VALUES (1, 1, 2, 100);
COMMIT;
```

**4. ACID Guarantee:**
- Atomicity: All-or-nothing with BEGIN/COMMIT
- Consistency: Foreign keys and check constraints
- Isolation: 2PL locking
- Durability: Write to stable storage on commit

---

## GATE-Style Previous Year Pattern Problems

### Problem 12.1 (GATE Type)
Relation schema with multiple candidate keys and overlapping FDs. Determine if BCNF or 3NF only.

### Problem 12.2 (GATE Type)
ER diagram with multiple M:N relationships and weak entities. Count minimum tables needed.

### Problem 12.3 (GATE Type)
Complex SQL with nested subqueries and GROUP BY involving multiple tables with NULL values.

### Problem 12.4 (GATE Type)
Precedence graph analysis with 3+ transactions and conflicting operations.

### Problem 12.5 (GATE Type)
B+ tree insertion sequence with specific order value, asking final tree structure.

---

## Answer Verification Checklist

**Normalization:**
- [ ] Checked for multi-valued attributes (1NF)
- [ ] Checked for partial dependencies (2NF)
- [ ] Checked for transitive dependencies (3NF)
- [ ] Verified functional dependencies

**ER to Relational:**
- [ ] Strong entities → relations
- [ ] 1:1 relationships → handled correctly
- [ ] 1:N relationships → foreign key placed
- [ ] M:N relationships → junction table
- [ ] Weak entities → include owner's key

**SQL:**
- [ ] COUNT(*) vs COUNT(attribute)
- [ ] GROUP BY includes all non-aggregate columns
- [ ] NULL handling
- [ ] JOIN conditions correct

**Concurrency:**
- [ ] Precedence graph built correctly
- [ ] Cycles checked
- [ ] Lock compatibility verified
- [ ] Timestamp ordering applied properly

Good luck with practice!
