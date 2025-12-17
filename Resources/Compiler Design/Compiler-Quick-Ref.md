# Compiler Design - GATE 2026 Quick Reference

## Lexical Analysis - Quick Facts

| Concept | Definition |
|---------|-----------|
| **Token** | Category of lexeme (keyword, identifier, operator) |
| **Lexeme** | Actual character sequence |
| **Attribute** | Value associated with token |
| **Regex** | Pattern for matching lexemes |
| **DFA** | Deterministic machine, single transition per symbol |
| **NFA** | Non-deterministic, multiple transitions possible |

**Common Regex Patterns:**
- Identifier: `[a-zA-Z][a-zA-Z0-9]*`
- Integer: `[0-9]+`
- Real: `[0-9]+\.[0-9]+`
- Whitespace: `[ \t\n]+`

---

## Parsing - Comparison Table

| Parser Type | Lookahead | Power | Table Size | Conflict Resolution |
|-----------|-----------|-------|-----------|-------------------|
| **LL(1)** | 1 | Weakest | Small | First/Follow sets |
| **SLR(1)** | 1 | Medium | Medium | FOLLOW sets |
| **LALR(1)** | 1 | Strong | Medium | Context-specific |
| **LR(1)/CLR** | 1 | Strongest | Large | Lookahead items |

**LL Parser: Top-down, reads left-to-right, leftmost derivation**
**LR Parser: Bottom-up, reads left-to-right, rightmost derivation**

### Grammar Transformation

**Remove Left Recursion:**
```
A → Aα | β  becomes:
A → βA'
A' → αA' | ε
```

**Left Factoring:**
```
A → αβ | αγ  becomes:
A → αA'
A' → β | γ
```

---

## Three-Address Code Forms

| Form | Structure | Example |
|------|-----------|---------|
| **Assignment** | a = b op c | a = b + c |
| **Unary** | a = op b | a = -b |
| **Copy** | a = b | a = x |
| **Jump** | goto L | goto label1 |
| **Conditional** | if a goto L | if x > 0 goto L1 |
| **Procedure** | call p, n | call func, 2 |
| **Return** | return x | return result |

### Quadruples: (operator, arg1, arg2, result)
```
For: a = b + c * d

(*, c, d, t1)    // t1 = c * d
(+, b, t1, t2)   // t2 = b + t1
(=, t2, _, a)    // a = t2
```

### Triples: Reference by instruction number
```
(0): (*, c, d)
(1): (+, b, (0))
(2): (=, a, (1))
```

---

## Data Flow Analysis Equations

### Reaching Definitions (Forward)
```
in[B] = ∪ out[P]  (union of predecessors)
out[B] = Gen[B] ∪ (in[B] - Kill[B])
where Gen[B] = definitions generated in B
      Kill[B] = definitions overwritten in B
```

### Liveness Analysis (Backward)
```
out[B] = ∪ in[S]  (union of successors)
in[B] = Use[B] ∪ (out[B] - Def[B])
where Use[B] = variables used before definition
      Def[B] = variables defined
```

### Constant Propagation (Forward)
```
in[B] = ∩ out[P]  (intersection - must propagate)
out[B] = Gen[B] ∪ (in[B] - Kill[B])
```

### Available Expressions (Forward)
```
in[B] = ∩ out[P]  (intersection - on all paths)
out[B] = Gen[B] ∪ (in[B] - Kill[B])
```

---

## Shift-Reduce Conflicts

### Shift-Reduce Conflict
**When:** Parser can shift next symbol OR reduce using production

**Resolution:** 
- Shift by default (favors longer derivation)
- Based on precedence of operator

### Reduce-Reduce Conflict
**When:** Multiple productions can reduce

**Resolution:** 
- Grammar is ambiguous - cannot be resolved by lookahead alone

### Example
```
E → E + E | E * E | id

For: 1 + 2 + 3
At position "1 + 2 +" can shift + or reduce E+E
Precedence decides: left-associative → reduce
```

---

## Activation Records (Stack Frame)

### Contents (from bottom to top)
1. **Return address** - Where to return after call
2. **Control link** - Pointer to caller's frame
3. **Access link** - For nested scope access
4. **Parameters** - Function arguments
5. **Local variables** - Declared in function
6. **Temporaries** - Computed values

### Pointers
- **Frame Pointer (FP):** Start of current frame
- **Stack Pointer (SP):** Top of stack
- **Return Address:** Where to jump on return

### Example Call
```
f(x, y, z) {
  int a, b;
  ...
}

Stack Frame for f:
[Return address]
[Control link]
[Access link]
[Parameters: x, y, z]
[Local vars: a, b]
[Temporaries]
```

---

## Attribute Evaluation Schemes

### Synthesized Attributes
- Computed from children
- Bottom-up evaluation
- Can evaluate during parsing (one pass)

**Example:**
```
E → E₁ + T  { E.val = E₁.val + T.val }
T → num     { T.val = num.val }
```

### Inherited Attributes
- Obtained from parent/siblings
- Top-down evaluation
- Need multiple passes

**Example:**
```
D → T L { L.type = T.type }
L → L₁, id { L₁.type = L.type; id.type = L.type }
```

---

## Code Optimization Techniques

| Technique | Type | Benefit |
|-----------|------|---------|
| **Constant Folding** | Local | Compile-time evaluation |
| **Dead Code Elim** | Local/Global | Remove unused assignments |
| **CSE** | Local/Global | Reuse computed values |
| **Peephole** | Local | Pattern matching optimization |
| **Loop Invariant** | Global | Move constant outside loop |
| **Induction Var** | Global | Simplify loop variables |

### Common Subexpression Elimination
```
Original:
a = b * c + g
d = b * c * e

Optimized:
tmp = b * c
a = tmp + g
d = tmp * e
```

---

## GATE Formula Quick Reference

**Reaching Definitions:**
- Fixed-point: Repeat until convergence
- Merge: Union of all predecessors
- Transfer: (in - Kill) ∪ Gen

**Liveness:**
- Backward analysis
- Merge: Union of all successors
- Transfer: Use ∪ (out - Def)

**Constant Propagation:**
- Forward analysis
- Merge: Intersection of predecessors
- Track: (var, constant) pairs

**Available Expressions:**
- Forward analysis
- Merge: Intersection of predecessors
- For CSE: Reuse if available at entry

---

## Parsing Algorithm Decision Tree

```
Is grammar ambiguous?
├─ Yes → Remove ambiguity or declare precedence
└─ No → Check grammar type

Does grammar have left recursion?
├─ Yes → Remove left recursion
└─ No → Continue

For LL parsing: Needs LL(1) grammar
├─ No left recursion ✓
├─ Left-factored ✓
├─ Can use FIRST/FOLLOW ✓

For LR parsing: Accepts broader class of grammars
├─ SLR: Uses FOLLOW sets
├─ LALR: Uses specific lookahead (most practical)
└─ LR(1): Uses full lookahead (rare)
```

---

## Common GATE Question Patterns

### Pattern 1: Identify Conflicts
```
Build LR parsing table, find shift-reduce conflicts
```

### Pattern 2: Generate Three-Address Code
```
Given expression, generate quadruples/triples
```

### Pattern 3: Data Flow Analysis
```
Given CFG and statement, compute reaching/liveness
```

### Pattern 4: Code Optimization
```
Apply CSE or dead code elimination
```

### Pattern 5: Grammar Properties
```
Remove left recursion, check ambiguity
```

---

## Problem-Solving Checklist

### For Grammar Problems
- [ ] Check for ambiguity
- [ ] Check for left recursion
- [ ] Check for left factoring issues
- [ ] Verify it's LL(1) or LR(k)
- [ ] Construct parse tree if needed

### For Parsing Table Problems
- [ ] Build LR(0) items / LL table
- [ ] Check for conflicts
- [ ] Resolve shift-reduce (precedence)
- [ ] Verify table entries
- [ ] Trace through sample input

### For Code Generation
- [ ] Identify subexpressions
- [ ] Apply operator precedence
- [ ] Generate temporaries
- [ ] Write three-address code
- [ ] Verify result

### For Data Flow Analysis
- [ ] Define Gen and Kill sets
- [ ] Initialize in/out
- [ ] Apply equations iteratively
- [ ] Check for convergence
- [ ] Verify results

---

## Key Terminology

- **Backtracking:** Revisit decision (not done in LL/LR)
- **Handle:** RHS of production ready to reduce
- **Viable Prefix:** Prefix allowing valid reductions
- **Lookahead:** Future symbol(s) for parsing decision
- **Fixed Point:** Solution where equations stabilize
- **Basic Block:** Straight-line code, no branches
- **Dominance:** Node always reached via another
- **Def-Use Chain:** Definition to its uses

---

Print this sheet for exam revision!
