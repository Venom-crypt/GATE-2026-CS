# Databases Complete Study Guide - GATE 2026

## Part 1: Entity-Relationship Model (ER Model)

### 1.1 Introduction to ER Model

The Entity-Relationship (ER) Model is a conceptual data model used to represent the structure of a database at a high level of abstraction. It bridges the gap between real-world requirements and the actual database implementation.

**Purpose:**
- Provides a visual and logical representation of data
- Easy to understand for non-technical stakeholders
- Helps in database design and planning
- Facilitates communication between developers and domain experts

### 1.2 Components of ER Diagram

#### 1. Entity (Rectangle)
An entity is a distinguishable real-world object or thing that exists independently.

**Types of Entities:**
- **Strong Entity**: Can exist independently (has its own primary key)
  - Represented by single rectangle
  - Example: Student, Course, Department

- **Weak Entity**: Cannot exist independently, depends on another entity
  - Represented by double rectangle
  - Must have a partial key (discriminator)
  - Example: Dependent (depends on Employee)

**Entity Example:**
```
Student Entity with attributes:
- StudentID (primary key)
- Name
- Email
- DOB
```

#### 2. Attribute (Oval/Ellipse)
An attribute represents a property or characteristic of an entity.

**Types of Attributes:**

**a) Simple vs Composite**
- **Simple Attribute**: Cannot be divided (e.g., Age, StudentID)
- **Composite Attribute**: Can be divided (e.g., Name = FirstName + LastName)

**b) Single-valued vs Multi-valued**
- **Single-valued**: Has only one value (e.g., Age)
- **Multi-valued**: Can have multiple values (e.g., Phone numbers)
- Represented by double oval

**c) Stored vs Derived**
- **Stored Attribute**: Explicitly stored in database (e.g., DOB)
- **Derived Attribute**: Calculated from other attributes (e.g., Age from DOB)
- Represented by dashed oval

**d) Key Attribute**
- Uniquely identifies an entity instance
- Represented by underlined oval
- Example: StudentID

#### 3. Relationship (Diamond)
A relationship represents an association between entities.

**Relationship Properties:**
- Can have attributes (e.g., "Enrollment" relationship with "Grade" attribute)
- Can be between same entity type (recursive) or different types
- Can be identifying or non-identifying

**Types of Relationships:**
- **Identifying Relationship**: Weak entity depends on strong entity
  - Represented by double diamond
  - Example: Dependent is identified through Employee

- **Non-identifying Relationship**: Both entities can exist independently
  - Represented by single diamond
  - Example: Student enrolls in Course

### 1.3 Cardinality and Participation

**Cardinality Ratios** (How many entities on each side):

| Notation | Meaning | Example |
|----------|---------|---------|
| 1:1 | One-to-One | Person has one Passport |
| 1:N | One-to-Many | Department has many Employees |
| M:N | Many-to-Many | Students enroll in Courses |

**Participation Constraints** (Does each entity participate?):

**a) Total Participation (Double line)**
- Every entity must participate in relationship
- Example: Every Employee must work in a Department

**b) Partial Participation (Single line)**
- Some entities may not participate
- Example: Some Employees may not manage anyone

### 1.4 ER Model Examples

**Example 1: University Database**
```
STUDENT (1) --- (M) ENROLLMENT (M) --- (1) COURSE
Attributes:
- Student: StudentID, Name, Email
- Course: CourseID, CourseName, Credits
- Enrollment: Grade, Semester (relationship attributes)
```

**Example 2: Company Database**
```
DEPARTMENT (1) --- (N) EMPLOYEE
EMPLOYEE (1) --- (M) PROJECT
EMPLOYEE (1) --- (M) DEPENDENT (identifying relationship)
```

### 1.5 Weak Entity and Identifying Relationship

**Weak Entity Characteristics:**
- No primary key of its own
- Depends on owner (strong) entity for identification
- Has a partial key (discriminator)
- Must have total participation in identifying relationship

**Example: Dependent**
```
EMPLOYEE has many DEPENDENT
- Dependent partial key: DependentName
- Full identifier: (EmployeeID, DependentName)
- Weak entity must have total participation
```

### 1.6 Specialization and Generalization (Enhanced ER)

**Generalization (Bottom-up):**
- Combines similar entities into generalized entity
- Example: Car and Truck → Vehicle

**Specialization (Top-down):**
- Divides entity into specialized entities
- Example: Vehicle → Car, Truck, Bus

**Types of Specialization:**
- **Total Specialization**: Every entity must be specialized (circle with line)
- **Partial Specialization**: Some entities may not be specialized
- **Disjoint**: An entity is in only one specialized set
- **Overlapping**: An entity can be in multiple specialized sets

---

## Part 2: Relational Model

### 2.1 Introduction to Relational Model

The Relational Model organizes data into tables (relations) consisting of rows (tuples) and columns (attributes). Developed by E.F. Codd, it's the most widely used data model.

**Key Concept:**
- Data stored in two-dimensional tables
- Each relation = table with rows and columns
- Tuple = row (record)
- Attribute = column (field)

### 2.2 Components of Relational Model

**1. Relation (Table)**
- Set of tuples (rows)
- All tuples have same schema
- Order of tuples doesn't matter
- Order of attributes can vary

**2. Tuple (Row)**
- Single record in relation
- Represents one instance of entity
- All tuples in relation have same structure

**3. Attribute (Column)**
- Named data field
- Has domain (set of allowed values)
- All values in column from same domain

**4. Schema**
- Relation name + list of attributes with domains
- Example: STUDENT(StudentID: Integer, Name: String, Age: Integer)

**5. Instance**
- Actual data in relation at given time
- Can change with insertions, updates, deletions

### 2.3 Properties of Relations

**1. Atomic Values**
- Each cell contains single, indivisible value
- No nested relations or multi-valued attributes

**2. Uniqueness of Tuples**
- No duplicate rows
- Each tuple is unique

**3. No Ordering**
- Rows have no inherent order
- Can be accessed in any sequence

**4. Column Headers Unique**
- Each attribute name unique in relation
- But same attribute name can appear in different relations

### 2.4 Relational Schema and Keys

**Candidate Key:**
- Minimal set of attributes uniquely identifying tuple
- No subset can uniquely identify

**Primary Key:**
- Chosen candidate key
- Used as main identifier
- Must be NOT NULL and UNIQUE

**Foreign Key:**
- Attribute(s) referencing primary key in another relation
- Maintains referential integrity

**Super Key:**
- Set of attributes uniquely identifying tuple
- Can have extra attributes

**Example:**
```
STUDENT(StudentID, Name, Email, DepartmentID)
- Primary Key: StudentID
- Candidate Key: Email (assuming unique)
- Foreign Key: DepartmentID (references Department)
- Super Key: StudentID + Name (unnecessary extra)
```

### 2.5 Codd's Rules

**Rule 0: Foundation Rule**
- DBMS must manage database by relations

**Rule 1: Information Rule**
- All information represented in tables

**Rule 2: Guaranteed Access Rule**
- Every value accessible using table name, primary key, attribute name

**Rule 3: Systematic Treatment of NULL**
- NULL values supported consistently

**Rule 4: Active Online Catalog**
- Database schema accessible via SQL

**Rule 5: Comprehensive Data Sublanguage**
- Complete data manipulation language (SQL)

**Rules 6-12** cover additional features like security, transaction support, etc.

---

## Part 3: Relational Algebra

### 3.1 Introduction to Relational Algebra

Relational Algebra is a procedural query language using mathematical operations to retrieve and manipulate data. It forms the foundation of SQL query optimization.

**Characteristics:**
- Procedural (specifies how to retrieve data)
- Operates on relations, returns relations
- Operations can be composed (closure property)

### 3.2 Basic Operations (Unary and Binary)

#### Unary Operations (Single Relation Input):

**1. Selection (σ) - WHERE clause**
- Selects tuples (rows) satisfying condition
- Syntax: σ_(condition)(Relation)
- Returns rows, all columns
- Example: σ_(Age > 20)(STUDENT)

**2. Projection (π) - SELECT clause**
- Selects specific attributes (columns)
- Syntax: π_(Attribute1, Attribute2)(Relation)
- Returns all rows for selected columns
- Removes duplicates automatically

**Combined Example:**
```
π_(Name, Email)(σ_(Age > 20)(STUDENT))
= SELECT Name, Email FROM STUDENT WHERE Age > 20
```

**3. Rename (ρ)**
- Renames relation or attributes
- Syntax: ρ_(NewName)(Relation)
- Or: ρ_(NewName/OldName)(Relation) for attributes

#### Binary Operations (Two Relations Input):

**1. Union (∪)**
- Combines tuples from two relations
- Removes duplicates
- Both relations must have same schema
- Syntax: Relation1 ∪ Relation2

**Example:**
```
Engineering_Students ∪ Science_Students = All_Students
```

**2. Set Difference (−)**
- Tuples in first relation but not in second
- Same schema requirement
- Syntax: Relation1 − Relation2

**Example:**
```
All_Students − Science_Students = Engineering_Students
```

**3. Intersection (∩)**
- Common tuples in both relations
- Equivalent to: R1 − (R1 − R2)
- Same schema required

**4. Cartesian Product (×)**
- Cross product of two relations
- Creates all combinations of tuples
- Result has all attributes from both relations
- Syntax: Relation1 × Relation2
- Tuples produced: |R1| × |R2|

**Example:**
```
If STUDENT has 100 tuples and COURSE has 5 tuples,
STUDENT × COURSE has 500 tuples
```

### 3.3 Derived Operations

**1. Join (⨝)**
- Combines related tuples from two relations
- Types: Inner, Left, Right, Full Outer, Cross
- Most important operation in practice

**Natural Join:**
- Joins on common attributes
- Common attribute appears once in result
- Syntax: Relation1 ⨝ Relation2

**Equijoin (Theta Join with =):**
- Joins on condition with equality
- Syntax: Relation1 ⨝_(condition) Relation2

**Theta Join (General):**
- Uses any comparison operator (<, >, <=, >=, ≠)
- Syntax: R1 ⨝_(A<B) R2

**Outer Joins:**
- **Left Outer**: All tuples from left relation
- **Right Outer**: All tuples from right relation
- **Full Outer**: All tuples from both relations
- Unmatched tuples filled with NULL

**2. Division (÷)**
- Complex operation for queries like "For all X, find..."
- Syntax: Relation1 ÷ Relation2

**Example:**
```
Find students who have taken ALL courses offered.
Relation1: (StudentID, CourseID) - enrolled courses
Relation2: (CourseID) - all courses
Result: StudentID of students taking all courses
```

**3. Aggregation (GROUP BY, HAVING)**
- Not traditional relational algebra
- Functions: SUM, COUNT, AVG, MIN, MAX
- Syntax: G_(GroupAttributes) Agg_Func(Attribute)(Relation)

### 3.4 Expression Optimization

**Commutativity:**
- Selection: σ_(P1) (σ_(P2)(R)) = σ_(P2) (σ_(P1)(R))
- Union: R ∪ S = S ∪ R

**Pushing Selection Down:**
- σ_(Condition)(R1 ⨝ R2) = (σ_(Condition)(R1)) ⨝ R2
- Reduces data earlier, improves performance

**Projection Combination:**
- π_(A) (π_(A,B)(R)) = π_(A)(R)

---

## Part 4: Tuple Relational Calculus (TRC)

### 4.1 Introduction

Tuple Relational Calculus is a non-procedural query language specifying what data to retrieve without specifying how.

**Syntax:**
```
{t | P(t)}
```
- t = tuple variable
- P(t) = predicate/condition

### 4.2 Components

**1. Tuple Variables**
- Represent rows from relations
- Example: STUDENT(StudentID, Name, Age)
- Variable t ranges over STUDENT relation

**2. Predicates and Quantifiers**
- **Existential (∃)**: "There exists"
- **Universal (∀)**: "For all"
- Logical operators: ∧ (AND), ∨ (OR), ¬ (NOT)

### 4.3 Query Examples

**Example 1: Simple Query**
```
{t | t ∈ STUDENT ∧ t[Age] > 20}
= SELECT * FROM STUDENT WHERE Age > 20
```

**Example 2: Existential Quantifier**
```
{t | t ∈ STUDENT ∧ ∃u ∈ ENROLLMENT(t[StudentID] = u[StudentID])}
= SELECT * FROM STUDENT 
  WHERE StudentID IN (SELECT StudentID FROM ENROLLMENT)
```

**Example 3: Universal Quantifier**
```
{t | t ∈ STUDENT ∧ ∀u ∈ COURSE(∃v ∈ ENROLLMENT(t[StudentID]=v[StudentID] ∧ u[CourseID]=v[CourseID]))}
= SELECT * FROM STUDENT WHERE StudentID has taken ALL courses
```

### 4.4 Safe Queries

**Safe Query**: Produces finite result set, independent of database domain

**Conditions for Safety:**
1. All attribute references bound by relation membership
2. Existential quantifiers range over actual relations
3. No negation or disjunction without care

**Unsafe Query Example:**
```
{t | ¬(t ∈ STUDENT)}
Returns infinite set (all non-students)
```

---

## Part 5: SQL Queries

### 5.1 Basic SQL Structure

**SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY**

### 5.2 SELECT Statement

**Basic Syntax:**
```sql
SELECT [DISTINCT] column1, column2, ...
FROM table_name
WHERE condition
ORDER BY column_name [ASC|DESC];
```

**DISTINCT:**
- Removes duplicate rows
- Example: SELECT DISTINCT Department FROM STUDENT;

**Arithmetic in SELECT:**
```sql
SELECT Name, Age, Age*12 AS AgeInMonths FROM STUDENT;
```

### 5.3 WHERE Clause

**Conditions:**
- Comparison: =, <>, <, >, <=, >=
- Logical: AND, OR, NOT
- Pattern: LIKE (wildcard: %, _)
- Range: BETWEEN...AND
- List: IN
- NULL: IS NULL, IS NOT NULL

**Examples:**
```sql
WHERE Age > 20 AND Department = 'CSE'
WHERE Name LIKE 'A%' (starts with A)
WHERE Age BETWEEN 20 AND 25
WHERE Department IN ('CSE', 'IT', 'ECE')
WHERE Supervisor IS NULL
```

### 5.4 JOIN Operations

**INNER JOIN:**
```sql
SELECT s.Name, c.CourseName
FROM STUDENT s INNER JOIN ENROLLMENT e
ON s.StudentID = e.StudentID
INNER JOIN COURSE c ON e.CourseID = c.CourseID;
```

**LEFT OUTER JOIN:**
```sql
SELECT s.Name, c.CourseName
FROM STUDENT s LEFT JOIN ENROLLMENT e
ON s.StudentID = e.StudentID
LEFT JOIN COURSE c ON e.CourseID = c.CourseID;
```

**NATURAL JOIN:**
```sql
SELECT * FROM STUDENT NATURAL JOIN ENROLLMENT;
(Joins on common attributes automatically)
```

**Self Join:**
```sql
SELECT e1.Name, e2.Name
FROM EMPLOYEE e1, EMPLOYEE e2
WHERE e1.ManagerID = e2.EmployeeID;
```

### 5.5 Aggregate Functions

**COUNT, SUM, AVG, MIN, MAX**

```sql
SELECT COUNT(*) FROM STUDENT; (total rows)
SELECT COUNT(DISTINCT Department) FROM STUDENT; (unique departments)
SELECT SUM(Salary) FROM EMPLOYEE;
SELECT AVG(Age) FROM STUDENT WHERE Department = 'CSE';
SELECT MAX(Age), MIN(Age) FROM STUDENT;
```

### 5.6 GROUP BY and HAVING

**GROUP BY:**
- Groups rows by specified columns
- Used with aggregate functions

```sql
SELECT Department, COUNT(*) as StudentCount
FROM STUDENT
GROUP BY Department;
```

**HAVING:**
- Filters groups (like WHERE for groups)
- Applied after GROUP BY

```sql
SELECT Department, COUNT(*) as StudentCount
FROM STUDENT
GROUP BY Department
HAVING COUNT(*) > 50;
```

### 5.7 ORDER BY

**Sorts result set**

```sql
SELECT Name, Age FROM STUDENT
ORDER BY Age DESC, Name ASC;
```

### 5.8 Subqueries

**In WHERE Clause:**
```sql
SELECT * FROM STUDENT
WHERE StudentID IN (SELECT StudentID FROM ENROLLMENT WHERE Grade = 'A');
```

**Correlated Subquery:**
```sql
SELECT Name FROM STUDENT s1
WHERE Age > (SELECT AVG(Age) FROM STUDENT s2 WHERE s2.Department = s1.Department);
```

---

## Part 6: Integrity Constraints

### 6.1 Types of Integrity Constraints

**1. Domain Constraint**
- Specifies valid values for attributes
- Data type and format restrictions
- Example: Age INT (0-150)

**2. Key Constraint**
- Ensures uniqueness of primary key
- No NULL values in primary key
- Every tuple uniquely identifiable

**3. Entity Integrity Constraint**
- Primary key cannot be NULL
- Ensures each entity has unique identifier

**4. Referential Integrity Constraint**
- Foreign key must reference existing primary key
- OR be NULL
- Prevents "orphan" records
- Example: DepartmentID in STUDENT must exist in DEPARTMENT

**5. Functional Dependency**
- If two tuples have same value for X, they must have same value for Y
- Denoted: X → Y
- Basis for normalization

### 6.2 Enforcing Integrity

**In SQL:**
```sql
CREATE TABLE STUDENT (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL,
    Age INT CHECK (Age >= 18 AND Age <= 60),
    Email VARCHAR(100) UNIQUE,
    DepartmentID INT REFERENCES DEPARTMENT(DepartmentID)
);
```

**Constraint Types:**
- PRIMARY KEY
- UNIQUE
- NOT NULL
- CHECK
- FOREIGN KEY ... REFERENCES
- DEFAULT

---

## Part 7: Normalization and Normal Forms

### 7.1 Purpose of Normalization

**Goals:**
- Eliminate data redundancy
- Reduce update anomalies
- Improve data consistency
- Save storage space
- Enable efficient querying

**Anomalies to Avoid:**
- **Insertion Anomaly**: Cannot insert without existing data
- **Update Anomaly**: Must update multiple records for single logical update
- **Deletion Anomaly**: Deleting data removes unintended information

### 7.2 First Normal Form (1NF)

**Definition:** Relation is in 1NF if every attribute contains only atomic (single, indivisible) values.

**Requirements:**
- No multi-valued attributes
- No nested relations
- Each cell contains single value

**Example of Violating 1NF:**
```
STUDENT(StudentID, Name, Courses)
StudentID | Name      | Courses
1         | Alice     | [CSE101, CSE102, CSE201]  ← Multi-valued attribute

Correct 1NF:
STUDENT(StudentID, Name)
ENROLLMENT(StudentID, CourseID)
```

### 7.3 Second Normal Form (2NF)

**Requirements:**
1. Must be in 1NF
2. No partial dependencies
   - Non-key attributes must fully depend on entire primary key
   - Not just part of composite key

**Partial Dependency:**
- Non-key attribute depends on subset of composite key

**Example Violating 2NF:**
```
ENROLLMENT(StudentID, CourseID, StudentName, CourseName, Grade)
Primary Key: (StudentID, CourseID)

Partial Dependencies:
- StudentName depends only on StudentID (not on CourseID)
- CourseName depends only on CourseID (not on StudentID)

Correct 2NF:
STUDENT(StudentID, StudentName)
COURSE(CourseID, CourseName)
ENROLLMENT(StudentID, CourseID, Grade)
```

### 7.4 Third Normal Form (3NF)

**Requirements:**
1. Must be in 2NF
2. No transitive dependencies
   - Non-key attributes must depend directly on primary key
   - Not transitively through other non-key attributes

**Transitive Dependency:**
- X → Y and Y → Z, therefore X → Z (transitive)
- If Y is non-key, this is problematic

**Example Violating 3NF:**
```
EMPLOYEE(EmployeeID, Name, DepartmentID, DepartmentName)
Primary Key: EmployeeID

Transitive Dependency:
- EmployeeID → DepartmentID (OK)
- DepartmentID → DepartmentName (Problem: going through non-key)

Correct 3NF:
EMPLOYEE(EmployeeID, Name, DepartmentID)
DEPARTMENT(DepartmentID, DepartmentName)
```

### 7.5 Boyce-Codd Normal Form (BCNF)

**Most Strict Form**

**Requirement:**
- Every determinant must be a candidate key
- No non-trivial functional dependency A → B where A is not superkey

**Example:**
```
PROFESSOR_COURSE(ProfessorID, CourseID, Time)
Functional Dependency: CourseID → Time
But CourseID is not candidate key, causing BCNF violation

Solution: Separate into
COURSE_SCHEDULE(CourseID, Time)
PROFESSOR_COURSE(ProfessorID, CourseID)
```

**Note:**
- Every BCNF is 3NF
- But not every 3NF is BCNF
- Most databases normalized to 3NF in practice

### 7.6 Higher Normal Forms (4NF, 5NF)

**4NF:** No non-trivial multi-valued dependencies
**5NF:** Every join dependency is from candidate key

These are rarely used in practice.

---

## Part 8: File Organization and Indexing

### 8.1 File Organization Methods

#### 1. Heap File Organization

**Characteristics:**
- Records inserted in arbitrary order
- Can use any available block
- No ordering or structure
- Linear search required

**Operations:**
- **Insert**: O(1) - add to end of file
- **Delete**: O(n) - must search through file
- **Search**: O(n) - linear scan
- **Update**: O(n) - find then update

**Advantages:**
- Fast insertion
- Simple to implement

**Disadvantages:**
- Slow search and deletion
- Storage wastage (deleted record space)

#### 2. Sequential File Organization

**a) Pile File Method:**
- Records stored in order of insertion
- New records added at end
- No sorting maintained

**b) Sorted File Method:**
- Records sorted by primary key
- Maintains order during insertions (expensive)
- Faster search using binary search

**Operations:**
- **Insert**: O(n) - must maintain order
- **Delete**: O(log n) using binary search to find, then shift
- **Search**: O(log n) - binary search
- **Update**: O(log n)

**Advantages:**
- Efficient for range queries
- Sequential access fast

**Disadvantages:**
- Expensive insertions (must reorder)
- Space overhead for reorganization

### 8.2 Indexing

**Purpose:**
- Faster data retrieval
- Reduces disk I/O
- Enables efficient range queries

**Types of Indexes:**
- **Primary Index**: On primary key
- **Secondary Index**: On non-key attributes
- **Composite Index**: On multiple attributes
- **Dense Index**: Entry for every record
- **Sparse Index**: Entry for some records

### 8.3 B-Tree (Balanced Tree)

**Properties:**
- Balanced binary search tree (AVL-like)
- Order m: each node has at most m children
- All leaves at same level
- Minimum occupancy: ⌈m/2⌉

**Structure:**
- **Root**: 1 to m children
- **Internal Nodes**: ⌈m/2⌉ to m children
- **Leaf Nodes**: ⌈m/2⌉ to m keys

**Features:**
- Record pointers in every node (including internal)
- No redundant keys
- Better for random access

**Search Operation:**
```
1. Start at root
2. Compare key with node keys
3. Follow appropriate branch
4. Continue until leaf reached
5. Record found in leaf or not found
```

**Insertion:**
- Find correct leaf node
- Insert key
- If node full, split node
- Propagate split upward

**Time Complexity:**
- Search: O(log n)
- Insert: O(log n)
- Delete: O(log n)

### 8.4 B+ Tree (Most Common in Databases)

**Key Differences from B-Tree:**
- Leaf nodes contain actual data, internal nodes only keys
- Leaf nodes linked for sequential access
- Redundant keys in internal nodes for routing
- Better for range queries

**Structure:**
```
        [25]
       /    \
    [10]     [40]
    /  \     /  \
   [5]  [15][30][45]
   ||   ||  ||  ||
   [Data links]
```

**Advantages over B-Tree:**
- More index entries fit in memory (no data pointers in internal nodes)
- Efficient sequential scanning via leaf node links
- Range queries optimized
- Consistent search time (always to leaf level)

**Characteristics:**
- **Order m**: Internal nodes can have m children
- **Leaf nodes linked**: Enables sequential access
- **Data only in leaves**: Smaller internal nodes
- **All leaves same level**: Balanced

**Operations:**
- **Search**: Go to leaf node → O(log n)
- **Range Search**: Find starting leaf, follow links → O(log n + result_size)
- **Insert**: Like B-tree, but data only in leaves
- **Delete**: From leaf node only

**Example B+ Tree (Order 3):**
- Each internal node: 1-2 keys, 2-3 children
- Each leaf node: 1-2 keys, data pointers

**B+ vs B Comparison:**
| Feature | B-Tree | B+ Tree |
|---------|--------|---------|
| Data location | Every node | Leaf only |
| Redundant keys | No | Yes |
| Sequential access | No | Yes (via links) |
| Range queries | Less efficient | Very efficient |
| Memory usage | More | Less (for same data) |
| Used in | Some systems | Most databases (MySQL, PostgreSQL) |

---

## Part 9: Transactions and ACID Properties

### 9.1 What is a Transaction?

A transaction is a sequence of operations (reads and writes) on database that must be executed atomically. Represents single logical unit of work.

**Transaction Example:**
```
Transfer $100 from Account A to Account B:
1. Read balance of A
2. Subtract 100 from A
3. Write new balance to A
4. Read balance of B
5. Add 100 to B
6. Write new balance to B

All must complete together (atomically)
If step 3 fails, must not execute steps 4-6
```

### 9.2 ACID Properties

#### A - Atomicity ("All or Nothing")

**Definition:** Transaction either executes completely or not at all.

**Guarantee:**
- If system crashes, either transaction is committed or rolled back
- No partial updates

**Implementation:**
- Logging (Write-Ahead Logging)
- Transaction logs stored before commit
- If failure, redo or undo using logs

**Example Violation:**
```
Transfer A to B:
- A = 900 (updated)
- System crashes before B update
Result: Money disappeared (database inconsistent)
```

#### C - Consistency

**Definition:** Transaction transforms database from one valid state to another valid state.

**Guarantee:**
- Integrity constraints maintained before and after
- All business rules satisfied

**Responsibility:**
- DBMS enforces declared constraints
- Application programmer ensures business logic

**Example:**
```
Total Money Before = Total Money After (conservation rule)
```

#### I - Isolation

**Definition:** Concurrent transactions don't interfere with each other.

**Guarantee:**
- Each transaction acts as if executing alone
- Other transactions invisible until commit
- Prevents dirty reads, lost updates, etc.

**Isolation Levels:**
1. **Serializable**: Highest isolation, slow
2. **Repeatable Read**: Prevents non-repeatable reads
3. **Read Committed**: Reads only committed data
4. **Read Uncommitted**: Lowest isolation, fast

**Implementation:**
- Locking (pessimistic)
- Timestamp ordering (optimistic)

#### D - Durability

**Definition:** Once committed, transaction effects are permanent.

**Guarantee:**
- Even if system failure, data recovers

**Implementation:**
- Stable storage (disk)
- Recovery protocols
- Backup and replication

---

## Part 10: Concurrency Control

### 10.1 Problems in Concurrent Execution

**1. Dirty Read**
```
T1: Read X = 100
T2: Update X = 200
T1: Read X = 200 (dirty read - T2 might abort)
T2: Abort (X back to 100, but T1 has 200)
```

**2. Lost Update**
```
T1: Read X = 100
T2: Read X = 100
T1: Update X = 150
T2: Update X = 120
Result: T1's update lost (should be 150 or 120?)
```

**3. Non-repeatable Read**
```
T1: Read X = 100
T2: Update X = 200
T1: Read X = 200 (different value in same transaction)
```

**4. Phantom Read**
```
T1: SELECT COUNT(*) FROM STUDENT WHERE Age > 20 (result: 50)
T2: INSERT student with Age = 25
T1: SELECT COUNT(*) FROM STUDENT WHERE Age > 20 (result: 51)
```

### 10.2 Lock-Based Concurrency Control

**Concept:**
- Transaction must acquire lock before accessing data
- Lock types: Shared (S) or Exclusive (X)

**Lock Compatibility:**
```
           | Request S | Request X |
-----------|-----------|-----------|
Hold S     | Yes       | No        |
Hold X     | No        | No        |
Hold None  | Yes       | Yes       |
```

**Lock Operations:**
- **Lock-S(X)**: Acquire shared lock on X
- **Lock-X(X)**: Acquire exclusive lock on X
- **Unlock(X)**: Release lock on X

### 10.3 Two-Phase Locking (2PL)

**Rule:**
- All lock requests before any unlock request
- Growing phase: Transaction acquires locks
- Shrinking phase: Transaction releases locks

**Example:**
```
Growing Phase:
Lock-S(A)
Lock-S(B)
Read A, Read B
Lock-X(C)
Update C

Shrinking Phase:
Unlock(A)
Unlock(B)
Unlock(C)
```

**Variants:**
- **Conservative 2PL**: Acquires all locks initially
- **Strict 2PL**: Holds exclusive locks until commit
- **Rigorous 2PL**: Holds all locks until commit

**Guarantees:**
- Conflict serializable schedules
- Prevents dirty reads, lost updates, non-repeatable reads

**Problem:** Can cause deadlocks

### 10.4 Deadlock Prevention and Detection

**Wait-Die Scheme (Non-preemptive):**
- Older transaction (lower timestamp) waits
- Younger transaction dies (rolls back)
- Prevents cycles

```
If TS(Ti) < TS(Tj) (Ti older, Tj younger):
  Ti requesting lock held by Tj: Ti waits
  Tj requesting lock held by Ti: Tj dies
```

**Wound-Wait Scheme (Preemptive):**
- Older transaction wounds younger
- Younger transaction waits for older
- Prevents cycles

```
If TS(Ti) < TS(Tj) (Ti older, Tj younger):
  Ti requesting lock held by Tj: Tj is wounded (rolls back)
  Tj requesting lock held by Ti: Tj waits
```

**Deadlock Detection:**
- Build wait-for graph
- Cycle indicates deadlock
- Resolve by aborting transaction

### 10.5 Timestamp Ordering Concurrency Control

**Approach:**
- Assign unique timestamp to each transaction
- Order determined before execution
- Check timestamps on read/write operations

**Basic Timestamp Ordering:**
- Each data item X has:
  - R-TS(X): Read timestamp (last transaction to successfully read)
  - W-TS(X): Write timestamp (last transaction to successfully write)

**Rules:**
- If TS(Ti) < R-TS(X): Ti reading data written after Ti started (impossible)
  - Abort Ti
- If TS(Ti) < W-TS(X): Ti writing data already written by later transaction
  - Abort Ti

**Thomas' Write Rule:**
- Modification: If TS(Ti) < W-TS(X), ignore the write (blind write)
- Improves concurrency without affecting serializability

---

## Part 11: Serializability

### 11.1 Serial vs Concurrent Schedules

**Serial Schedule:**
- Transactions execute one after another
- No interleaving of operations
- Always consistent but slow

**Concurrent Schedule:**
- Transactions interleaved
- Faster but can cause problems
- Must ensure equivalence to serial schedule

### 11.2 Conflict Serializability

**Conflicting Operations:**
- Two operations conflict if:
  1. Different transactions
  2. Same data item
  3. At least one is write

**Non-conflicting Operations:**
- Can be reordered without changing result

**Conflict Serializability:**
- Schedule S is conflict serializable if can be transformed to serial schedule by swapping non-conflicting operations

**Precedence Graph:**
- Node for each transaction
- Edge from Ti to Tj if Ti's operation must come before Tj's operation
- No cycles = Conflict serializable

**Example:**
```
Schedule: T1:R(A), T2:W(A), T1:R(B), T2:W(B)

Conflicts:
- T1:R(A) and T2:W(A) - edge T1→T2
- T1:R(B) and T2:W(B) - edge T1→T2

Graph: T1 → T2 (no cycle = serializable)
Equivalent serial: T1, T2
```

### 11.3 View Serializability

**More general than conflict serializability**

**View Equivalence:**
- Two schedules equivalent if:
  1. Initial read same (who first reads each item)
  2. Updated read same (who reads written data)
  3. Final write same (who finally writes)

**Blind Write:**
- Write without reading first
- Can pass view serializability but not conflict

**Example:**
```
T1: W(A)
T2: W(A)  ← Blind write, overwrites T1
T3: R(A)

Final write by T2, so order T1, T2, T3 or T2, T1, T3 valid
(whichever has T2 after T1 in view sense)
```

**Checking:**
- More expensive computationally
- Rarely used in practice

---

## Part 12: Functional Dependencies and Decomposition

### 12.1 Functional Dependencies

**Definition:** X → Y if whenever two tuples have same value for X, they have same value for Y.

**Notation:**
- LHS (Left): Determinant
- RHS (Right): Dependent

**Examples:**
```
StudentID → Name, Email, Age
(StudentID determines all attributes)

CourseID → CourseName
(Course ID determines course name)
```

### 12.2 Types of Functional Dependencies

**Trivial FD:**
- X → Y where Y ⊆ X
- Always true
- Not interesting

**Non-trivial FD:**
- At least one attribute in Y not in X

**Useful FD (GATE Definition):**
- X is non-empty
- Y is non-empty
- X ∩ Y is empty

### 12.3 Armstrong's Axioms

**1. Reflexivity:**
- If Y ⊆ X, then X → Y

**2. Augmentation:**
- If X → Y, then XZ → YZ

**3. Transitivity:**
- If X → Y and Y → Z, then X → Z

**Derived Rules:**
- **Union**: If X → Y and X → Z, then X → YZ
- **Decomposition**: If X → YZ, then X → Y and X → Z
- **Pseudo-transitivity**: If X → Y and YZ → W, then XZ → W

### 12.4 Closure of Attributes

**Closure of X (X+):**
- Set of all attributes determined by X

**Algorithm:**
```
result = X
while (result changes):
    for each FD A → B:
        if A ⊆ result:
            result = result ∪ B
```

**Example:**
```
FDs: A → B, B → C
Find A+:
- Start: result = {A}
- A → B: result = {A, B}
- B → C: result = {A, B, C}
- A+ = {A, B, C}
```

### 12.5 Decomposition

**Lossless Decomposition:**
- R decomposed into R1 and R2
- Natural join of R1 and R2 equals R
- No information lost

**Condition:**
- (R1 ∩ R2) → (R1 - R2) or
- (R1 ∩ R2) → (R2 - R1)

**Dependency Preserving:**
- Union of FDs from R1 and R2 equals original FDs
- Can enforce original constraints on decomposed relations

**Example:**
```
R(A, B, C) with A → B
Decompose to R1(A, B) and R2(A, C)
- Lossless: A (common) determines B
- Dependency preserving: A → B remains in R1
```

---

## Study Strategy for GATE

### Phase 1: Foundation (Weeks 1-2)
- ER Model and diagrams
- Relational Model basics
- Normal forms (1NF, 2NF, 3NF, BCNF)

### Phase 2: Query Languages (Weeks 3-4)
- Relational Algebra: all operations
- SQL: SELECT, JOIN, GROUP BY
- Tuple Calculus basics

### Phase 3: Design and Optimization (Weeks 5-6)
- File organization and indexing
- B and B+ trees
- ER to relational conversion

### Phase 4: Transactions and Concurrency (Weeks 7-8)
- ACID properties
- Concurrency control
- Serializability

### Phase 5: Practice and Revision (Weeks 9-10)
- Practice problems from all topics
- Previous year questions
- Mock exams

---

## High-Weightage Topics Summary

1. **Normal Forms** - 20% of exam
2. **ER to Relational Conversion** - 15%
3. **SQL Queries** - 12%
4. **Relational Algebra** - 12%
5. **Concurrency Control** - 10%
6. **B+ Trees** - 8%
7. **Transactions (ACID)** - 8%
8. **Functional Dependencies** - 8%
9. **Other topics** - 7%

Total = 100%
