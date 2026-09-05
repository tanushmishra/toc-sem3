# TOC Test 1 — Complete Solutions

---

## Q1a) Strings of length ≤ 2 over Σ = {x, y, z} [1 Mark]

| Length | Strings | Count |
|---|---|---|
| 0 (empty) | ε | 1 |
| 1 | x, y, z | 3 |
| 2 | xx, xy, xz, yx, yy, yz, zx, zy, zz | 9 |

$$\text{Total} = |\Sigma|^0 + |\Sigma|^1 + |\Sigma|^2 = 1 + 3 + 9 = \boxed{13}$$

---

## Q1b) Prefixes & Suffixes of "abaabbab" up to length 3 [1 Mark]

String: **a b a a b b a b**

**Prefixes** (from the start):
| Length | Prefix |
|---|---|
| 0 | ε |
| 1 | a |
| 2 | ab |
| 3 | aba |

**Suffixes** (from the end):
| Length | Suffix |
|---|---|
| 0 | ε |
| 1 | b |
| 2 | ab |
| 3 | bab |

---

## Q2) Compute L1·L2* ∪ L3* [2 Marks]

Given: **L1 = {ε}, L2 = {a}, L3 = {∅}** (empty language)

**Step 1:** Compute L2\*
$$L2^* = \{\varepsilon, a, aa, aaa, \ldots\} = a^*$$

**Step 2:** Compute L1·L2\*
$$L1 \cdot L2^* = \{\varepsilon\} \cdot a^* = \varepsilon \cdot \{\varepsilon, a, aa, \ldots\} = \{\varepsilon, a, aa, aaa, \ldots\} = a^*$$

**Step 3:** Compute L3\*
$$L3^* = \emptyset^* = \{\varepsilon\}$$
*(Kleene star of the empty language = {ε}, since we can concatenate zero strings)*

**Step 4:** Union
$$L1 \cdot L2^* \cup L3^* = a^* \cup \{\varepsilon\} = a^*$$
*(since ε already belongs to a*)*

$$\boxed{L1 \cdot L2^* \cup L3^* = a^* = \{\varepsilon, a, aa, aaa, \ldots\}}$$

---

## Q3) DFA: at least one 'a' AND exactly two 'b's [2 Marks]

**Strategy:** Track (# of b's seen, whether any 'a' seen) simultaneously.

**States:**

| State | b-count | 'a' seen? | Accept? |
|---|---|---|---|
| q₀₀ | 0 | No | No |
| q₀₁ | 0 | Yes | No |
| q₁₀ | 1 | No | No |
| q₁₁ | 1 | Yes | No |
| q₂₀ | 2 | No | No |
| **q₂₁** | **2** | **Yes** | **✅ YES** |
| q_d | 3+ | — | No (dead) |

**Transition Table:**

| State | on **a** | on **b** |
|---|---|---|
| q₀₀ (start) | q₀₁ | q₁₀ |
| q₀₁ | q₀₁ | q₁₁ |
| q₁₀ | q₁₁ | q₂₀ |
| q₁₁ | q₁₁ | q₂₁ |
| q₂₀ | q₂₁ | q_d |
| **q₂₁ ✅** | q₂₁ | q_d |
| q_d | q_d | q_d |

**Transition Diagram:**
```
        a              a              a
q₀₀ ──────► q₀₁    q₁₀ ──────► q₁₁    q₂₀ ──────► q₂₁ ((accept))
 │            │      │            │      │                  │
 │b           │b     │b           │b     │b                 │b
 ▼            ▼      ▼            ▼      ▼                  ▼
q₁₀ ───────► q₁₁   q₂₀ ───────► q₂₁   q_d ◄────────────── q_d
       a                   a           (dead, self-loops on a,b)
```
> Start: q₀₀ | Accept: {q₂₁} | 7 states total

---

## Q4) NFA: third symbol from right is always 'a' [2 Marks]

**Language:** All strings over {a,b} where the 3rd symbol from the right end is 'a'
**Regular Expression:** `(a+b)*a(a+b)(a+b)`

**Strategy:** Non-deterministically "guess" when we are at the 3rd-from-last position.

**States & Transitions:**

| State | on **a** | on **b** | Accept? |
|---|---|---|---|
| q₀ (start) | {q₀, q₁} | {q₀} | No |
| q₁ | {q₂} | {q₂} | No |
| q₂ | {q₃} | {q₃} | No |
| q₃ | ∅ | ∅ | **✅ YES** |

**Diagram:**
```
        a,b
        ┌──┐
  a     ▼  │   a,b        a,b
→(q₀)──────►(q₁)──────►(q₂)──────►((q₃))
   └──────►                              
     a (also goes to q₀)

Precise:
q₀ on a → q₀ AND q₁   (NFA branches)
q₀ on b → q₀
q₁ on a or b → q₂
q₂ on a or b → q₃
q₃ = accepting, no transitions
```

> The NFA guesses (non-deterministically) that the current 'a' is 3rd from right, then must read exactly 2 more symbols before accepting.

---

## Q5 (Page 1) — Construct Minimized DFA [4 Marks]

**Reading the given DFA:**

| State | on **0** | on **1** | Accept? |
|---|---|---|---|
| A (start→) | B | D | No |
| B | B | C | No |
| C | E | F | No |
| D | E | F | No |
| E | E | F | **✅ YES** |
| F | F | F | **✅ YES** |

*(F has self-loop on 0,1; E has self-loop on 0)*

### Step 1: Remove inaccessible states
All states A, B, C, D, E, F are reachable. ✓

### Step 2: Initial partition
- **Group I (Accepting):** {E, F}
- **Group II (Non-Accepting):** {A, B, C, D}

### Step 3: Mark distinguishable pairs

**Base marking** (one accepting, one not):
Mark: (A,E), (A,F), (B,E), (B,F), (C,E), (C,F), (D,E), (D,F) ✓

**Iterate on unmarked pairs:** (A,B), (A,C), (A,D), (B,C), (B,D), (C,D), (E,F)

| Pair | on 0 leads to | Marked? | on 1 leads to | Marked? | Decision |
|---|---|---|---|---|---|
| (E,F) | (E,F) | No | (F,F) | No | **NOT marked** → E ≡ F |
| (C,D) | (E,E) | No | (F,F) | No | **NOT marked** → C ≡ D |
| (A,C) | (B,E) | **YES** | — | — | **MARKED** → distinguishable |
| (A,D) | (B,E) | **YES** | — | — | **MARKED** |
| (B,C) | (B,E) | **YES** | — | — | **MARKED** |
| (B,D) | (B,E) | **YES** | — | — | **MARKED** |
| (A,B) | (B,B) | No | (D,C)=(C,D) | No | **NOT marked** → A ≡ B |

### Step 4: Equivalence Classes

$$\{A, B\} \quad \{C, D\} \quad \{E, F\}$$

### Step 5: Minimized DFA (3 states!)

Rename: **P = {A,B}**, **Q = {C,D}**, **R = {E,F}**

| State | on **0** | on **1** | Accept? |
|---|---|---|---|
| **P** (start) | P | Q | No |
| **Q** | R | R | No |
| **R** | R | R | **✅ YES** |

*(Since A:0→B∈P, B:0→B∈P → P:0→P; A:1→D∈Q, B:1→C∈Q → P:1→Q)*
*(Since C:0→E∈R, D:0→E∈R → Q:0→R; C:1→F∈R, D:1→F∈R → Q:1→R)*

```
     0           0,1
  ┌──┐    1    ┌──┐    0,1   ┌──────┐
→(P)──────────►(Q)──────────►((R))  │
  └──┘         └──┘          └──────┘
   │                              ▲
   └──────────────────────────────┘
              0 (P→P self-loop)
```

> **Original 6 states → Minimized 3 states**

---

## Q5 (Page 2) — Construct DFA from ε-NFA [4 Marks]

**Given ε-NFA:**
- States: q₀, q₁, q₂, q₃, q₄, q₅, q₆, q₇
- Start: q₀ | Final: **{q₆, q₇}**
- Transitions:
  - q₀ →**ε**→ q₁ →**ε**→ q₂
  - q₁: 0,1 **self-loop**
  - q₂: **0**→q₃, **1**→q₄
  - q₃: **0**→q₅ | q₄: **1**→q₅
  - q₅ →**ε**→ q₆ →**ε**→ q₇
  - q₆: 0,1 **self-loop**

### ε-Closures

| State | ε-closure |
|---|---|
| q₀ | {q₀, q₁, q₂} |
| q₁ | {q₁, q₂} |
| q₂ | {q₂} |
| q₃ | {q₃} |
| q₄ | {q₄} |
| q₅ | {q₅, q₆, q₇} |
| q₆ | {q₆, q₇} |
| q₇ | {q₇} |

### Subset Construction

**DFA Start State** = ε-closure(q₀) = **{q₀,q₁,q₂}** → call it **A**

| DFA State | NFA States | on **0** → | on **1** → | Accept? |
|---|---|---|---|---|
| **A** (start) | {q₀,q₁,q₂} | **B** | **C** | No |
| **B** | {q₁,q₂,q₃} | **D** | **C** | No |
| **C** | {q₁,q₂,q₄} | **B** | **E** | No |
| **D** ✅ | {q₁,q₂,q₃,q₅,q₆,q₇} | **D** | **F** | YES |
| **E** ✅ | {q₁,q₂,q₄,q₅,q₆,q₇} | **G** | **E** | YES |
| **F** ✅ | {q₁,q₂,q₄,q₆,q₇} | **G** | **E** | YES |
| **G** ✅ | {q₁,q₂,q₃,q₆,q₇} | **D** | **F** | YES |

**Sample computation (A on 0):**
- q₀ on 0: ∅; q₁ on 0: {q₁}; q₂ on 0: {q₃}
- Union → {q₁,q₃}; ε-closure → {q₁,q₂,q₃} = **B** ✓

> **Language recognized:** (0+1)\*(00+11)(0+1)\* — strings containing "00" or "11"

---

## Q6a) Design Mealy Machine — outputs A/B/C [4 Marks]

**Specification:**
- Output **A** if last two inputs end with "**10**"
- Output **B** if last two inputs end with "**11**"
- Output **C** otherwise

**States:** Track only the last input symbol.

| State | Meaning |
|---|---|
| q₀ | Start / last input was '0' (or start) |
| q₁ | Last input was '1' |

**Mealy Transition Table (State, Input / Output → Next State):**

| Current State | Input | Output | Next State |
|---|---|---|---|
| q₀ | 0 | **C** | q₀ |
| q₀ | 1 | **C** | q₁ |
| q₁ | 0 | **A** | q₀ |
| q₁ | 1 | **B** | q₁ |

**Diagram:**
```
         0/C          0/A
        ┌──┐          ┌──┐
        │  │   1/C    │  │
  start ▼  │  ──────► ▼  │  1/B
       (q₀)          (q₁)◄──┘
        └──────────────┘
              0/A (q₁ on 0 → q₀)
```

**Verification:**
- Input "10": q₀ →(1/C)→ q₁ →(0/**A**)→ q₀ ✓
- Input "11": q₀ →(1/C)→ q₁ →(1/**B**)→ q₁ ✓
- Input "01": q₀ →(0/C)→ q₀ →(1/**C**)→ q₁ ✓

---

## Q6b) Convert Mealy Machine → Moore Machine [4 Marks]

**Given Mealy Machine:**

| State | Input | Output | Next State |
|---|---|---|---|
| q₀ (start) | 0 | z₁ | q₂ |
| q₀ | 1 | z₁ | q₃ |
| q₂ | 0 | z₁ | q₃ |
| q₂ | 1 | z₁ | q₂ |
| q₃ | 0 | z₂ | q₃ |
| q₃ | 1 | z₁ | q₂ |

### Step 1: Find incoming outputs for each state

| Mealy State | Incoming transitions (with output) | Outputs arriving |
|---|---|---|
| q₀ | (start — no incoming) | — (use z₁ by default) |
| q₂ | q₀→q₂/z₁, q₂→q₂/z₁, q₃→q₂/z₁ | Only **z₁** |
| q₃ | q₀→q₃/z₁, q₂→q₃/z₁, q₃→q₃/z₂ | **z₁** and **z₂** → SPLIT! |

### Step 2: Create Moore states (split states with multiple incoming outputs)

| Moore State | Derived from | Output λ |
|---|---|---|
| q₀ (start) | q₀ | z₁ |
| q₂ | q₂ (only z₁ arrives) | z₁ |
| q₃ᴬ | q₃ (when arriving via z₁ output) | z₁ |
| q₃ᴮ | q₃ (when arriving via z₂ output) | z₂ |

### Step 3: Build Moore transitions

Use original Mealy transitions, replacing destinations with the appropriate split state:

| Moore State | on **0** → | on **1** → | λ(output) |
|---|---|---|---|
| q₀ (start) | q₂ *(goes to q₂ with z₁)* | q₃ᴬ *(goes to q₃ with z₁)* | z₁ |
| q₂ | q₃ᴬ *(goes to q₃ with z₁)* | q₂ *(goes to q₂ with z₁)* | z₁ |
| q₃ᴬ | q₃ᴮ *(goes to q₃ with z₂)* | q₂ *(goes to q₂ with z₁)* | z₁ |
| q₃ᴮ | q₃ᴮ *(goes to q₃ with z₂)* | q₂ *(goes to q₂ with z₁)* | z₂ |

**Moore Machine Diagram:**
```
         0              0              0
        ──────         ──────         ────
[q₀/z₁]──────►[q₂/z₁]──────►[q₃ᴬ/z₁]──────►[q₃ᴮ/z₂]
   │              │  ◄──────    │              │  ◄─────
   │1             │    1        │1             │    0
   ▼              ▼             ▼              │(self-loop)
[q₃ᴬ/z₁]        [q₂/z₁]      [q₂/z₁]        │
                                              │1
                                              ▼
                                           [q₂/z₁]
```

**Moore Machine Summary:**
- **4 states:** q₀, q₂, q₃ᴬ, q₃ᴮ
- **Output function** λ assigns output to each state (not transition)
- **Key difference from Mealy:** output depends only on current state, produced on entering the state

---

*Paper: TOC Test 1 | All solutions verified from Class Lecture Slides*
