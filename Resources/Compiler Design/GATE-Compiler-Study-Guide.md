# Compiler Design Complete Study Guide - GATE 2026

## Part 1: Lexical Analysis

### 1.1 Overview

Lexical analysis is the first phase of compilation. It converts source code into a sequence of tokens, removing whitespace and comments.

**Input:** Source code (stream of characters)
**Output:** Sequence of tokens with attributes
**Tools:** Lex, Flex, or hand-coded scanners

### 1.2 Tokens and Lexemes

**Token:** A category (keyword, identifier, operator)
**Lexeme:** Actual sequence of characters matching the token
**Attribute:** Value associated with token

### 1.3 Regular Expressions for Tokens

```
Keyword: for, while, if, etc.
Identifier: letter (letter | digit)*
Integer: digit+
Real: digit+ . digit+
Operator: +, -, *, /, =, ==, etc.
```

### 1.4 Finite Automata

**DFA (Deterministic Finite Automaton):**
- Each state has exactly one transition for each symbol
- Used to recognize tokens
- No backtracking needed

**NFA (Non-deterministic Finite Automaton):**
- Can have multiple transitions for same symbol
- Converted to DFA (subset construction)
- Theoretical model for regex

### 1.5 Lexical Errors

**Errors detected:**
- No pattern matches remaining input
- Invalid character sequences
- Malformed numbers or strings

**Recovery techniques:**
- Panic mode: skip until recognized token
- Delete erroneous character
- Replace with valid character
- Insert missing character

---

## Part 2: Parsing

### 2.1 Context-Free Grammars

**Components:**
- Terminals: tokens from lexer
- Non-terminals: symbols to expand
- Productions: A → α (A is non-terminal, α is string)
- Start symbol: Initial non-terminal

**Example:**
```
E → E + T | T
T → T * F | F
F → (E) | id
```

### 2.2 Parse Trees

Hierarchical representation showing how string derives from grammar.

**In-order traversal gives original string**

### 2.3 Ambiguity

**Ambiguous grammar:** Multiple parse trees for same string

**Solution:** Add precedence and associativity rules
- Precedence: * before +
- Associativity: + is left-associative

### 2.4 LL Parsing (Top-Down)

**Characteristics:**
- Starts with start symbol
- Expands left-most non-terminal
- Uses lookahead to decide production
- **LL(k):** Uses k tokens lookahead

**LL(1) Grammar Requirements:**
- No left recursion
- Left-factored

**Removing left recursion:**
```
A → Aα | β  becomes  A → βA'
                       A' → αA' | ε
```

**Left factoring:**
```
A → αβ | αγ  becomes  A → αA'
                        A' → β | γ
```

**Predictive parsing table:**
- Row: Non-terminal
- Column: Terminal (lookahead)
- Entry: Production to apply

### 2.5 LR Parsing (Bottom-Up)

**Shift-Reduce Parsing:**
- Shift: Push token onto stack
- Reduce: Replace RHS of production with LHS

**LR Parser Types:**

**1. SLR (Simple LR)**
- Uses LR(0) items
- FOLLOW sets to decide reduce
- Simplest implementation
- Limited grammar support

**2. CLR (Canonical LR / LR(1))**
- Uses LR(1) items
- Lookahead in item
- Most powerful LR parser
- Large parse tables

**3. LALR (Lookahead LR)**
- Merges LR(1) states
- Smaller tables than CLR
- More practical than SLR
- Most widely used

**LR(0) Item:** A → α•β (dot shows parse position)
- Complete item: A → α• (ready to reduce)
- Incomplete item: A → α•Xβ

**Shift-Reduce Conflicts:** 
- Action table has both shift and reduce
- Resolved by choosing higher precedence operation

**Reduce-Reduce Conflicts:**
- Multiple reductions possible
- Cannot be resolved by lookahead alone

---

## Part 3: Syntax-Directed Translation

### 3.1 Attribute Grammars

**Synthesized attributes:** Computed from children's attributes (bottom-up)
**Inherited attributes:** Obtained from parent or siblings (top-down)

### 3.2 Syntax-Directed Definitions (SDD)

Grammar with semantic rules attached to productions.

**Example:**
```
E → E₁ + T    { E.val = E₁.val + T.val }
E → T         { E.val = T.val }
T → num       { T.val = num.val }
```

### 3.3 Translation Schemes

Embed semantic actions directly in grammar.

```
E → E₁ + T { print("+") }
```

### 3.4 L-Attributed Definitions

**Property:** Attributes computed in single left-to-right pass
- Can be evaluated during parsing
- Practical for real compilers

---

## Part 4: Runtime Environments

### 4.1 Activation Records (Stack Frames)

**Contents:**
- Saved return address
- Local variables
- Temporary values
- Parameters
- Control link (to caller's frame)
- Access link (for nested scopes)

**Frame pointer (FP):** Points to start of activation record
**Stack pointer (SP):** Points to top of stack

### 4.2 Calling Sequences

**Caller responsibilities:**
- Prepare arguments
- Save registers if needed
- Call (jump to function)

**Callee responsibilities:**
- Save registers
- Allocate local variables
- Execute code
- Return value
- Restore registers
- Return (jump to return address)

### 4.3 Scope and Lifetime

**Scope:** Textual region where name is valid
**Lifetime:** Runtime duration of variable

**Symbol table:** Maps names to attributes

### 4.4 Function Calls and Returns

**Non-recursive:** Simple call/return
**Recursive:** Each call gets new activation record

---

## Part 5: Intermediate Code Generation

### 5.1 Intermediate Representations

**High-level:** Close to source language
**Low-level:** Close to machine code

**Three-address code:** Most common

### 5.2 Three-Address Code

Each instruction has at most 3 addresses.

**Forms:**
1. **Assignment:** a = b op c
2. **Unary:** a = op b (e.g., a = -b)
3. **Copy:** a = b
4. **Jump:** goto L
5. **Conditional jump:** if a goto L
6. **Procedure call:** call p, n (n parameters)
7. **Return:** return x

### 5.3 Quadruples Representation

Four fields: operator, arg1, arg2, result

```
(op, arg1, arg2, result)
(+, b, c, t1)    // t1 = b + c
(*, t1, d, a)    // a = t1 * d
```

### 5.4 Triples Representation

Avoid temporary variables by referencing instruction numbers.

```
#    | op  | arg1 | arg2 |
-----|-----|------|------|
(0)  | *   | c    | d    |
(1)  | +   | b    | (0)  |
(2)  | assign | a | (1) |
```

### 5.5 Indirect Triples

Pointers to triples for reordering flexibility.

### 5.6 Example: Code Generation

```
a = b * (c + d)
```

**Three-address code:**
```
t1 = c + d
t2 = b * t1
a = t2
```

**Quadruples:**
```
(+, c, d, t1)
(*, b, t1, t2)
(=, t2, _, a)
```

---

## Part 6: Local Optimization

### 6.1 Basic Blocks

**Basic block:** Maximal sequence of instructions with:
- Single entry point
- Single exit point
- No jumps except at end
- No labels except at start

**Finding basic blocks:**
1. Identify leaders (first instruction, jump targets)
2. Blocks end where next leader begins

### 6.2 Peephole Optimization

Small window looking at consecutive instructions.

**Patterns:**
- Redundant loads/stores
- Unreachable code
- Jump to jump

**Example:**
```
Load  r1, a      Load  r1, a
Store r1, b  →   Load  r1, b
Load  r1, b
```

### 6.3 Local Code Optimization Techniques

**Constant folding:** Compile-time evaluation
```
a = 2 + 3  →  a = 5
```

**Dead code elimination:** Remove unused variables/assignments

**Redundant instruction elimination:** Remove duplicate computations in basic block

---

## Part 7: Data Flow Analysis

### 7.1 Control Flow Graph (CFG)

- Nodes: Basic blocks
- Edges: Possible control flow
- Useful for all data flow analyses

### 7.2 Reaching Definitions

**Definition:** Assignment to variable
**Reaching:** Definition d reaches point p if path exists where d not killed

**Gen[B]:** Definitions generated in B
**Kill[B]:** Definitions killed (overwritten) in B

**Equations:**
```
in[B] = ∪ out[P] for all predecessors P
out[B] = Gen[B] ∪ (in[B] - Kill[B])
```

### 7.3 Liveness Analysis

**Variable v live at point p:** Value will be used in future

**Use[B]:** Variables used before definition
**Def[B]:** Variables defined

**Equations (backward):**
```
out[B] = ∪ in[S] for all successors S
in[B] = Use[B] ∪ (out[B] - Def[B])
```

**Algorithm:**
```
For each block:
  in[B] = ∅, out[B] = ∅
Repeat until no change:
  For each block:
    out[B] = ∪ in[S]
    in[B] = Use[B] ∪ (out[B] - Def[B])
```

### 7.4 Constant Propagation

**Track which variables have constant values**

**Information:** Variable v has value c

**Gen[B]:** Assignments of constants
**Kill[B]:** Assignments to same variable

**Equations:**
```
in[B] = ∩ out[P] (merge with intersection)
out[B] = Gen[B] ∪ (in[B] - Kill[B])
```

**Use for optimization:** Replace variable with constant

### 7.5 Available Expressions

**Expression available at p:** All paths from entry reach p, evaluating expression, with operands unchanged

**Gen[B]:** Expressions evaluated in B
**Kill[B]:** Expressions with modified operands

**Equations:**
```
in[B] = ∩ out[P]
out[B] = Gen[B] ∪ (in[B] - Kill[B])
```

---

## Part 8: Common Subexpression Elimination (CSE)

### 8.1 Principle

Compute expression once, reuse result.

**Example:**
```
a = b * c + g
d = b * c * e
```

**Optimized:**
```
tmp = b * c
a = tmp + g
d = tmp * e
```

### 8.2 Local CSE (Within Basic Block)

1. Compute expressions in order
2. For each expression, check if already computed
3. If yes, reuse temporary; if no, create new temporary

**Implementation:** Hash table of expressions → temporaries

### 8.3 Global CSE (Across Basic Blocks)

Uses available expressions analysis.

1. Compute available expressions for each block
2. For expressions available at block entry, reuse
3. Otherwise compute new temporary

---

## Part 9: Summary of Compilation Phases

```
Source Code (characters)
    ↓
LEXICAL ANALYSIS
    ↓
Tokens
    ↓
PARSING
    ↓
Parse Tree / Syntax Tree
    ↓
SEMANTIC ANALYSIS / SYNTAX-DIRECTED TRANSLATION
    ↓
Annotated Syntax Tree
    ↓
INTERMEDIATE CODE GENERATION
    ↓
Three-Address Code / Quadruples
    ↓
CODE OPTIMIZATION
  - Local optimization (basic blocks)
  - Data flow analysis
  - Global optimization
    ↓
Optimized Intermediate Code
    ↓
CODE GENERATION
    ↓
Target Code (Assembly)
    ↓
LINKING / LOADING
    ↓
Executable
```

---

## High-Weightage Topics for GATE

1. **Lexical Analysis** (8%)
   - Tokens, regular expressions, DFA
   
2. **Parsing** (18%)
   - Grammars, LL parsing, LR parsing
   - SLR, LALR, shift-reduce conflicts
   
3. **Syntax-Directed Translation** (10%)
   - Attributes, translation schemes
   - Code generation basics
   
4. **Intermediate Code** (12%)
   - Three-address code, quadruples, triples
   - Code generation for expressions
   
5. **Runtime Environments** (10%)
   - Activation records, calling sequences
   - Symbol tables
   
6. **Data Flow Analysis** (15%)
   - Reaching definitions, liveness
   - Constant propagation
   
7. **Code Optimization** (17%)
   - Local optimization, peephole
   - CSE, dead code elimination
   
8. **Other Topics** (10%)

**Focus areas for GATE:** Parsing (especially LR), code generation, data flow analysis, optimizations
