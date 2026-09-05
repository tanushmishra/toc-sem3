# 📘 Theory of Computation — Test 1 Study Notes
> Units 1, 2, 3 | Based on your class lecture slides + supplementary materials

---

## Unit 1 — General Introduction

### 1.1 Branches of Theory of Computation (ToC)

| Branch | Concerned With | Tools |
|---|---|---|
| **Automata Theory** | Modeling machines & recognizing languages | DFA, NFA, PDA, TM |
| **Computability Theory** | What *can* be computed? | Recursive functions, TMs |
| **Complexity Theory** | How *efficiently* can it be done? | Time/space complexity |

> [!NOTE]
> Undecidable problems: Halting Problem, Solving integer equations (Hilbert's 10th), Post Correspondence Problem

### 1.2 Key Definitions

**Alphabet (Σ):** A *finite* set of symbols.
- e.g., Σ = {0,1}, Σ = {a,b,…,z}

**String:** A *finite* sequence of symbols from an alphabet.
- **Empty string:** ε (or λ) — length 0
- **Length:** |w|, e.g., |0110| = 4, |ε| = 0
- **Concatenation:** if ω = abd, α = ce → ωα = abdce
- **Exponentiation:** ω³ = abdabdabd, ω⁰ = ε
- **Reversal:** ω = abd → ω^R = dba
- **Prefix / Suffix / Substring:** parts of a string

**Special string sets:**
- **Σᵏ** = all strings of exactly length k over Σ
- **Σ\*** = Σ⁰ ∪ Σ¹ ∪ Σ² ∪ … = all possible strings (including ε)
- **Σ⁺** = Σ\* − {ε} = all non-empty strings

**Formal Language:** Any subset L ⊆ Σ\*
- Can be finite or infinite

### 1.3 Operations on Languages

| Operation | Definition |
|---|---|
| Union | L₁ ∪ L₂ = {x \| x ∈ L₁ or x ∈ L₂} |
| Intersection | L₁ ∩ L₂ = {x \| x ∈ L₁ and x ∈ L₂} |
| Difference | L₁ − L₂ = {x \| x ∈ L₁ and x ∉ L₂} |
| Complement | L̄ = Σ\* − L |
| Concatenation | L₁L₂ = {xy \| x ∈ L₁, y ∈ L₂} |
| Kleene Star | L\* = L⁰ ∪ L¹ ∪ L² ∪ … |
| Positive Closure | L⁺ = L¹ ∪ L² ∪ … = L\* − {ε} |
| Reverse | L^R = {w^R \| w ∈ L} |

### 1.4 Mathematical Preliminaries

**Sets:**
- Powerset of S: 2^S — all subsets; \|2^S\| = 2^|S|
- If \|A\| = n → number of relations on A = 2^(n²)

**Relations:**
- Binary relation R ⊆ A × B
- Inverse: flip pairs — if R = {(2,3), (4,−7)} → R⁻¹ = {(3,2), (−7,4)}
- Composition: S ◦ R — if (a,b) ∈ R and (b,c) ∈ S then (a,c) ∈ S ◦ R

**Mathematical Induction:**
- Basis Step: prove P(1) (or P(base))
- Inductive Step: assume P(k), prove P(k+1)

**Proof Techniques:** Induction, Contrapositive, Contradiction, Counter-example

### 1.5 Automata Hierarchy (Chomsky)

```
Finite Automata   ← least power
Pushdown Automata
Turing Machine    ← most power
```

---

## Unit 2 — Finite Automata & Regular Languages

### 2.1 Deterministic Finite Automata (DFA)

**Formal definition:** M = (Q, Σ, δ, q₀, F) where:
- **Q** = finite set of states
- **Σ** = finite input alphabet
- **δ : Q × Σ → Q** = transition function (total; exactly one move per state/symbol)
- **q₀ ∈ Q** = start state
- **F ⊆ Q** = set of final/accepting states

**Key property:** δ is a *function* — exactly one arc per symbol per state. At each step, exactly one computation path.

**Extended transition function δ̂:**
- δ̂(q, ε) = q
- δ̂(q, wa) = δ(δ̂(q, w), a)
- Tells you the state after reading a full string w from state q.

**Language of DFA:** L(M) = {w ∈ Σ\* | δ̂(q₀, w) ∈ F}

**DFA Design Examples from lectures:**
| Language | Key idea |
|---|---|
| Strings with even number of 0s | 2 states: even-count (accept), odd-count |
| Strings containing at least two c's | 3 states: 0c, 1c, ≥2c (accept) |
| Strings of length ≥ 2 | 3 states: q₀ → q₁ → q₂ (accept), q₂ loops |
| Not containing substring "aa" | Dead state on "aa" |
| Containing substring "aba" | 4 states tracking progress |

> [!IMPORTANT]
> A DFA partitions Σ\* into two sets: **L(M)** (accepted) and **Σ\* − L(M)** (rejected).
> Some languages (e.g., L = {0ⁿ1ⁿ | n ≥ 0}) are **NOT** regular.

### 2.2 Non-deterministic Finite Automata (NFA)

**Formal definition:** M = (Q, Σ, δ, q₀, F) where:
- **δ : Q × Σ → 2^Q** (maps to the *powerset* of Q — can return ∅, or multiple states)

**Key differences from DFA:**
1. From one state, multiple (or zero) transitions allowed on same symbol
2. λ/ε-moves are possible (ε-NFA)
3. A string is **accepted** if *at least one* computation path leads to a final state

**Is NFA more powerful than DFA?** ❌ NO — NFA = DFA in power.

### 2.3 ε-NFA (NFA with ε-transitions)

- Transitions labeled ε (epsilon) are "free" moves — no input symbol consumed
- **ε-closure(q)** = set of states reachable from q using only ε-transitions

**ε-NFA to NFA conversion:** Compute ε-closures, then eliminate ε-transitions.

### 2.4 NFA → DFA Conversion (Subset Construction)

Given NFA M = (Q, Σ, δ, q₀, F), construct DFA N = (Q′, Σ, δ′, q₀′, F′):

| Component | Definition |
|---|---|
| Q′ | All subsets of Q (power set 2^Q) |
| q₀′ | {q₀} (or ε-closure({q₀}) for ε-NFA) |
| F′ | All sets in Q′ that contain at least one final state from F |
| δ′({q₁,q₂,...,qₖ}, a) | δ(q₁,a) ∪ δ(q₂,a) ∪ ... ∪ δ(qₖ,a) |

**Algorithm:**
1. Start with {q₀} as the initial state of DFA
2. For each new DFA state (a set of NFA states) and each input symbol, compute the union of NFA transitions
3. Create new DFA states as needed
4. ∅ (empty set) = dead/trap state (non-accepting, loops on all inputs)
5. Any DFA state containing an NFA final state → becomes a DFA final state

> [!TIP]
> In the worst case, NFA with n states → DFA with 2ⁿ states, but often far fewer are reachable.

### 2.5 DFA Minimization

**Goal:** Find the DFA with the *fewest* states accepting L(M).

**Distinguishable states:** p and q are **distinguishable** if there exists a string w such that exactly one of δ̂(p,w), δ̂(q,w) is in F.

**Indistinguishable states:** p and q where for all w: δ̂(p,w) ∈ F iff δ̂(q,w) ∈ F.

**The "Mark" Procedure:**
1. Remove all inaccessible states
2. Mark all pairs (p, q) where one is in F and other is not → **distinguishable**
3. Repeat: for all pairs (p,q) and all a ∈ Σ, if (δ(p,a), δ(q,a)) is marked → mark (p,q)
4. Stop when no new pairs are marked
5. Remaining unmarked pairs = **indistinguishable states** → merge them

**The "Reduce" Procedure:**
1. Use Mark to partition Q into sets of indistinguishable states
2. Each partition set → one state in reduced DFA
3. Transfer transitions accordingly
4. Initial state = partition containing q₀
5. Final states = partitions containing any f ∈ F

**Theorem (Minimality):** The reduced DFA M′ is minimal — no DFA with fewer states accepts L(M).

### 2.6 Moore Machines & Mealy Machines

Both are **finite state transducers** (produce output).

| Feature | Moore Machine | Mealy Machine |
|---|---|---|
| Output depends on | State only | State AND input |
| Output timing | On entering a state | On each transition |
| Formal | M = (Q, Σ, Δ, δ, λ, q₀) | M = (Q, Σ, Δ, δ, λ, q₀) |
| λ (output function) | λ: Q → Δ | λ: Q × Σ → Δ |
| Output length | \|w\| + 1 (one per state visited) | \|w\| (one per transition) |

**Interconversion:**
- **Moore → Mealy:** For each transition (q, a) → p, output λ_Moore(p)
- **Mealy → Moore:** Split states — for each state q with different outputs for different inputs, create copies of q (one per distinct output)

### 2.7 Regular Operations & Closure Properties

Regular languages are **closed** under these operations (result is always regular):

| Operation | Proof Method |
|---|---|
| Union (L₁ ∪ L₂) | RE: R₁+R₂; or NFA construction |
| Concatenation (L₁L₂) | RE: R₁R₂ |
| Kleene Star (L\*) | RE: R\* |
| Complement (L̄) | DFA: swap accepting/non-accepting states |
| Intersection (L₁ ∩ L₂) | L₁ ∪ L₂ (using De Morgan); or Cross-Product DFA |
| Difference (L₁ − L₂) | L₁ ∩ L₂̄ |
| Reverse (L^R) | Reverse DFA transitions |
| Homomorphism h(L) | Replace each symbol via RE |
| Inverse Homomorphism h⁻¹(L) | DFA simulation of h(w) |

**Cross-Product Construction (Intersection):**
Given DFAs M₁ = (Q₁, Σ, δ₁, q₁, F₁) and M₂ = (Q₂, Σ, δ₂, q₂, F₂):
- Q = Q₁ × Q₂ (pairs of states)
- q₀ = ⟨q₁, q₂⟩
- δ(⟨p₁, p₂⟩, a) = ⟨δ₁(p₁,a), δ₂(p₂,a)⟩
- F = F₁ × F₂ (both must be accepting)

> [!IMPORTANT]
> **Proving regularity:** Build a DFA/NFA/RE, OR show it's obtained from known regular languages via closure operations.
> **Proving non-regularity:** Use Pumping Lemma, OR show that a known non-regular language can be derived from it via closure operations.

---

## Unit 3 — Regular Expressions

### 3.1 Definition of Regular Expressions

**Base cases:**
- **a** (for any a ∈ Σ): RE for language {a}
- **ε**: RE for language {ε}
- **∅**: RE for language {} (empty language)

**Inductive cases:** If R₁ and R₂ are REs, then so are:
- **R₁ + R₂** (or R₁ | R₂): Union — L(R₁ + R₂) = L(R₁) ∪ L(R₂)
- **R₁R₂**: Concatenation — L(R₁R₂) = L(R₁) · L(R₂)
- **R\***: Kleene star — L(R\*) = (L(R))\*

### 3.2 Operator Precedence (highest → lowest)

1. `*` (Kleene star / closure)
2. Concatenation
3. `+` (union)

> Parentheses override precedence.

### 3.3 Common RE Examples (from your lectures)

| RE | Language Described |
|---|---|
| `01*` | {0, 01, 011, 0111, …} |
| `(0+1)*` | All strings of 0s and 1s (including ε) |
| `(0+1)*00(0+1)*` | Strings containing "00" |
| `(0+1)*011` | Strings ending with "011" |
| `0*1*` | Strings with no "0" after a "1" |
| `00*11*` | At least one 0 followed by at least one 1, no 0 after 1 |
| `(1+10)*` | Strings starting with "1" containing no "00" |
| `(0+1)*1(0+1)(0+1)` | Strings where 3rd-from-last symbol is 1 |
| `(1*01*01*01*)*` | Number of 0s is a multiple of 3 |
| `a*` | All combinations of a's (including ε) |
| `a+` | All combinations of a's (excluding ε) |
| `(a+b)*` | Any strings of a's and b's |
| `(0+1)*00` | Strings ending with "00" |
| `1(1+0)*0` | Strings starting with 1 and ending with 0 |
| `(0+1)*(00+11)(0+1)*` | Strings containing "00" or "11" |

**RE for time (12hr clock):**
```
((0+1+…+9) + 1(0+1+2)) : ((0+1+…5)(0+1+…9)) (am+pm)
```

### 3.4 Equivalence of Regular Expressions

Two REs R₁ and R₂ are **equivalent** if **L(R₁) = L(R₂)**.

**Key algebraic identities:**
| Identity | Rule |
|---|---|
| R + ∅ = R | ∅ is identity for union |
| Rε = εR = R | ε is identity for concatenation |
| R∅ = ∅R = ∅ | ∅ annihilates concatenation |
| R\*\* = R\* | Star is idempotent |
| (R + S)\* = (R\* S\*)\* = (R\* + S\*)\* | Star distributes over union |
| ε + RR\* = R\* | R+ = RR\* |
| R + R = R | Union is idempotent |

### 3.5 RE ↔ Automata Equivalence

**Theorem:** DFA = NFA = ε-NFA = RE (all describe exactly the **Regular Languages**)

**RE → ε-NFA (Thompson's Construction):**

For each base case:
- `∅`: single state, no transitions, non-accepting
- `ε`: start state → (ε) → accepting state
- `a`: start state → (a) → accepting state

For inductive cases:
- **R₁ + R₂:** New start q₀ → (ε) → [M_R₁] → (ε) → new accept q₁; same for M_R₂
- **R₁R₂:** New start q₀ → (ε) → [M_R₁] → (ε) → [M_R₂] → (ε) → new accept q₁
- **R\*:** New start q₀ → (ε) → [M_R] → (ε) → new accept q₁; plus back-loop ε and direct ε from q₀ to q₁

**FA → RE:** Use state elimination method (covered in class — the lecture mentions it but the textual extraction was limited for diagram-heavy slides).

---

## Quick Reference: Types of Automata

| Machine | Memory | Language Class |
|---|---|---|
| DFA / NFA / ε-NFA | None (finite states only) | Regular Languages |
| Pushdown Automaton (PDA) | Stack | Context-Free Languages |
| Turing Machine | Unlimited R/W tape | Recursively Enumerable Languages |

---

## Exam Strategy Tips (Based on T1 PYP Analysis)

> [!NOTE]
> The PYP (`TOC_T1.pdf`) is a scanned PDF — text couldn't be extracted. Questions in typical T1 papers follow these patterns:

**Most likely question types for Units 1–3:**

1. **Draw DFA/NFA** for a given language description *(very frequent)*
2. **NFA → DFA conversion** (Subset Construction) *(guaranteed)*
3. **DFA Minimization** (Mark algorithm) *(almost always)*
4. **Write Regular Expression** for a given language *(frequent)*
5. **Prove/disprove regularity** using closure properties *(medium frequency)*
6. **Moore/Mealy design or interconversion** *(medium frequency)*
7. **Formal definition** of DFA/NFA/RE *(short answer)*
8. **Set/language operations** – compute Σ\*, L\*, concatenation, etc. *(Unit 1 theory)*

**Common traps to avoid:**
- ε is NOT the same as ∅ (empty language ≠ language with empty string)
- In NFA→DFA: ∅ state is a **dead trap state** — it still needs transitions for all symbols (self-looping)
- In DFA minimization: first **remove inaccessible states** before running Mark
- Moore output length = |w| + 1; Mealy output length = |w|
- Kleene star of ∅ = {ε} (because ∅⁰ = {ε})

---

*Generated from: Lect 1–3, Lect 4–11, Lect 12–17 (Class slides) + unit2_Closure Properties PDF*
