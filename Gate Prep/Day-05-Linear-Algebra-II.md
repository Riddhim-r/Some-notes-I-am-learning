# GATE DA · Day 5 of 20
## Linear Algebra II: eigenvalues, special matrices, quadratic forms, SVD

*About a 30-minute read. Answers to the practice and recall questions are in the collapsible section at the bottom.*

---

## 1. How GATE asks this

This half of linear algebra feeds straight into ML: PCA is eigenvectors of a covariance matrix, and least squares is a projection. GATE questions are mostly short computations built on properties:
- **NAT:** an eigenvalue of f(A), the trace or determinant of some matrix expression, a singular value, the rank of a projection.
- **MSQ:** "which statements about this idempotent, orthogonal or symmetric matrix are true?" and "which matrices are diagonalisable?"
- **MCQ:** whether a quadratic form is positive definite, or what a projection matrix does.

Almost none of it needs heavy algebra if you know the **eigenvalue shortcuts** in 2.2.

---

## 2. The ideas from zero

### 2.1 Eigenvalues and eigenvectors: the intuition

Most vectors change direction when you multiply them by A. An **eigenvector** is a special nonzero vector that A only **stretches** (or flips, or shrinks), without turning:

**Av = λv, with v ≠ 0.**

λ is the **eigenvalue**: the stretch factor along that direction.

**Picture it with A = [[2, 1], [1, 2]].** A turns the unit circle into an ellipse. Along the two eigen-directions, points just slide in or out without turning:

```
        unit circle                       after A: ellipse
                                                        ↗ (1,1) stretched ×3
             ↗ (1,1)                               . ·
          .-''-.                              . ·      .
         /      \            A               .        ·
        |   •    |       ─────────▶        .    •    .
         \      /                         ·        .
          '-..-'                         .      · ·
             ↘ (1,−1)                       · ·  ↘ (1,−1) unchanged ×1
```

| Vector v | Av | What happened |
|---|---|---|
| (1, 1) | (3, 3) | same direction, ×3 → eigenvector, λ = 3 |
| (1, −1) | (1, −1) | same direction, ×1 → eigenvector, λ = 1 |
| (1, 0) | (2, 1) | **turned** → not an eigenvector |

A is symmetric, so its eigenvectors are perpendicular and its SVD equals its eigendecomposition. For a non-symmetric matrix, the ellipse's axes (singular vectors) and the eigenvectors are different directions.

**How to find them:**
- Av = λv ⇔ (A − λI)v = 0 has a nonzero solution ⇔ **det(A − λI) = 0.** This is the **characteristic equation**: a degree-n polynomial, so there are n eigenvalues counted with multiplicity (possibly complex).
- For each λ, the eigenvectors are the nonzero vectors in **N(A − λI)**. That null space is the **eigenspace**.

**2×2 shortcut:** λ² − (tr A)λ + det A = 0.

**3×3:** λ³ − (tr A)λ² + (sum of the principal 2×2 minors)λ − det A = 0.

### 2.2 The shortcuts (very high yield)

For an n×n A with eigenvalues λ₁…λₙ:
- **Σλᵢ = trace(A).**
- **∏λᵢ = det(A).** So A is singular ⇔ 0 is an eigenvalue.
- **Triangular or diagonal matrix:** the eigenvalues are the **diagonal entries.**
- **A and Aᵀ** have the same eigenvalues (the eigenvectors may differ).
- **If Av = λv,** then for the **same eigenvector v:**
  - Aᵏ → λᵏ
  - A⁻¹ → **1/λ** (if A is invertible)
  - A + kI → **λ + k**
  - cA → cλ
  - Any polynomial: p(A) → **p(λ)**. For example, A² − 3A + 2I → λ² − 3λ + 2.
  - adj(A) → det(A)/λ
- **Similar matrices** (B = P⁻¹AP) have the same eigenvalues, trace, determinant and rank.
- **AB and BA** have the same nonzero eigenvalues.
- **Rank-1 matrix uvᵀ:** the eigenvalues are vᵀu (= trace) and 0 (with multiplicity n − 1). For a matrix of all 1s (n×n): the eigenvalues are n and 0 (n−1 times).
- **Eigenvectors for distinct eigenvalues are linearly independent.**
- **Cayley–Hamilton:** every matrix satisfies its own characteristic equation. For a 2×2, A² − (tr A)A + (det A)I = 0. Use it to get A⁻¹ or high powers without heavy multiplication.

### 2.3 Multiplicity and diagonalisation

- **Algebraic multiplicity (AM):** how many times λ is a root of the characteristic polynomial.
- **Geometric multiplicity (GM):** dim N(A − λI) = n − rank(A − λI). It's the number of independent eigenvectors for λ.
- **1 ≤ GM ≤ AM, always.**

**Diagonalisable:** A = PDP⁻¹, where D is the diagonal matrix of eigenvalues and P has the matching eigenvectors as columns.
- **A is diagonalisable ⇔ GM = AM for every eigenvalue** ⇔ A has n independent eigenvectors.
- **n distinct eigenvalues ⇒ diagonalisable** (sufficient, not necessary).
- **Every real symmetric matrix is diagonalisable.**
- **Defective example:** [[2, 1], [0, 2]] has λ = 2 with AM 2 but GM 1. It is **not** diagonalisable.
- **Payoff:** Aᵏ = PDᵏP⁻¹, so powers are trivial.

### 2.4 The spectral theorem: symmetric matrices

If A is real and **symmetric** (Aᵀ = A):
- **All eigenvalues are real.**
- **Eigenvectors for different eigenvalues are orthogonal.**
- A is **orthogonally diagonalisable:** **A = QΛQᵀ**, with Q orthogonal (orthonormal eigenvectors as columns) and Λ diagonal.
- Equivalently, A = Σ λᵢ qᵢqᵢᵀ: a weighted sum of rank-1 projections onto the eigenvectors.

This is why covariance matrices (always symmetric) have real, orthogonal principal directions. That's all PCA is.

### 2.5 Special matrices

**Orthogonal Q:** QᵀQ = QQᵀ = I, so **Q⁻¹ = Qᵀ.**
- The columns (and rows) are orthonormal.
- It **preserves lengths and angles:** ‖Qx‖ = ‖x‖. Rotations and reflections are examples.
- **det Q = ±1.** Rotations have +1, reflections −1.
- **Every eigenvalue has |λ| = 1.** A 2D rotation by θ has eigenvalues e^(±iθ), which are complex unless θ = 0 or π.
- Products and inverses of orthogonal matrices are orthogonal.

**Idempotent P:** **P² = P.** Applying it twice does nothing new.
- **Eigenvalues are only 0 or 1.**
- **rank(P) = trace(P).**
- **I − P is also idempotent,** and P(I − P) = 0.
- An idempotent matrix is always diagonalisable.
- The only **invertible** idempotent is I.

**Projection matrices.** These are idempotent, and the orthogonal ones are also symmetric (P = Pᵀ = P²).
- **Onto a vector a:**
  - proj_a(b) = (aᵀb / aᵀa)·a
  - matrix **P = aaᵀ / (aᵀa)**: rank 1, trace 1, eigenvalues 1, 0, …, 0.
- **Onto the column space of A** (independent columns): **P = A(AᵀA)⁻¹Aᵀ.**
  - This is exactly the least-squares fit: Pb = Ax̂, where x̂ = (AᵀA)⁻¹Aᵀb.
  - The error b − Pb is orthogonal to C(A).
- **I − P** projects onto the **orthogonal complement.**
- rank P = trace P = the dimension of the subspace you project onto.

**Skew-symmetric (Aᵀ = −A):** the eigenvalues are 0 or purely imaginary. Odd order ⇒ det = 0.

**Nilpotent (Aᵏ = 0 for some k):** every eigenvalue is 0, so the trace is 0 and det is 0.

**Involutory (A² = I):** the eigenvalues are ±1.

**Partitioned (block) matrices.** Split a matrix into blocks and multiply block by block, as if the blocks were numbers (keeping the order, since blocks don't commute). This works as long as the sizes match.
- **Block diagonal** diag(A, D):
  - eigenvalues = those of A together with those of D
  - det = det A · det D
  - inverse = diag(A⁻¹, D⁻¹)
  - rank = rank A + rank D
- **Block triangular** [[A, B], [0, D]]:
  - det = det A · det D
  - eigenvalues = eig(A) ∪ eig(D)
- **Schur complement:** for M = [[A, B], [C, D]] with A invertible, det M = det A · det(D − CA⁻¹B).
- Transpose of a block matrix: transpose the block layout **and** each block.

### 2.6 Quadratic forms and definiteness

A **quadratic form** is Q(x) = xᵀAx, with A **symmetric**. Every degree-2 polynomial with no linear terms can be written this way.
- **Writing the matrix:** for Q = ax² + 2bxy + cy², A = [[a, b], [b, c]]. Put the **x² and y² coefficients on the diagonal**, and **half the xy coefficient** on each off-diagonal.
  - Example: 3x² + 4xy + y² → [[3, 2], [2, 1]].

**Definiteness of A (or of Q):**

| Type | Q(x) for x ≠ 0 | Eigenvalues |
|---|---|---|
| Positive definite (PD) | always > 0 | all > 0 |
| Positive semidefinite (PSD) | always ≥ 0 | all ≥ 0 |
| Negative definite | always < 0 | all < 0 |
| Indefinite | both signs | mixed signs |

**Sylvester's test for PD:** **every leading principal minor is > 0.** These are the determinants of the top-left 1×1, 2×2, …, n×n blocks.
- For a 2×2 [[a, b], [b, c]]: PD ⇔ a > 0 and ac − b² > 0.
- **Negative definite:** the leading minors **alternate** in sign, starting negative (−, +, −, …).
- **PSD needs ALL principal minors ≥ 0,** not just the leading ones.

**Quick facts:**
- **AᵀA is always PSD,** and it's PD if A has independent columns.
- Covariance matrices are PSD.
- A PD matrix is invertible, has a PD inverse, positive diagonal entries and det > 0.

**Link to optimisation (Day 6):**
- A **Hessian that's PD** at a critical point means a **local minimum.**
- Negative definite means a local maximum.
- Indefinite means a saddle point.

**Extremes on the unit sphere (Rayleigh quotient):**
- max of xᵀAx subject to ‖x‖ = 1 is λ_max.
- The minimum is λ_min.

### 2.7 Singular value decomposition (SVD)

**Every** real m×n matrix (square or not, full rank or not) can be factored as:

**A = UΣVᵀ**
- **V** (n×n, orthogonal): its columns vᵢ are the **right singular vectors** = the eigenvectors of **AᵀA.**
- **U** (m×m, orthogonal): its columns uᵢ are the **left singular vectors** = the eigenvectors of **AAᵀ.**
- **Σ** (m×n, diagonal): the **singular values** σ₁ ≥ σ₂ ≥ … ≥ 0, where **σᵢ = √(eigenvalues of AᵀA).**

**Geometry:** any linear map = rotate (Vᵀ) → stretch along the axes (Σ) → rotate (U). The unit sphere becomes an ellipsoid with semi-axes σᵢ along the uᵢ.

**Facts:**
- **rank A = number of nonzero singular values.**
- Avᵢ = σᵢuᵢ, so **uᵢ = Avᵢ/σᵢ.** Use this to build U from V.
- AᵀA and AAᵀ have the **same nonzero eigenvalues** (σᵢ²).
- **‖A‖₂ (spectral norm) = σ₁.**
- **Frobenius norm:** ‖A‖_F² = Σᵢⱼ aᵢⱼ² = **Σσᵢ².**
- Square A: **|det A| = ∏σᵢ.**
- **Symmetric PSD A:** SVD = eigendecomposition, with σᵢ = λᵢ.
- Symmetric but indefinite: σᵢ = |λᵢ|.
- **Orthogonal Q:** every σᵢ = 1.
- **Condition number** = σ_max/σ_min. Large means numerically unstable.
- **Pseudoinverse:** A⁺ = VΣ⁺Uᵀ, where Σ⁺ inverts the nonzero σᵢ. It gives the least-squares minimum-norm solution.

**Outer-product form:** A = Σᵢ σᵢuᵢvᵢᵀ, a sum of rank-1 pieces ordered by importance.

**Low-rank approximation (Eckart–Young):** keep the top k terms, Aₖ = Σᵢ≤ₖ σᵢuᵢvᵢᵀ. This is the **best** rank-k approximation in both the 2-norm and the Frobenius norm.
- Error in the 2-norm: ‖A − Aₖ‖₂ = σₖ₊₁.
- Error in the Frobenius norm: ‖A − Aₖ‖_F = √(σₖ₊₁² + … ). It's the basis of image compression and recommender systems.

**Link to PCA:**
- Centre the data matrix X (n samples × d features).
- The covariance is C = XᵀX/(n−1).
- The **principal components** are the eigenvectors of C, which are the **right singular vectors V of X.**
- The variance along PC i is λᵢ = σᵢ²/(n−1).
- Keeping the top-k components is the low-rank approximation of X. (Full detail on Day 17.)

---

## 3. Worked examples

**Example 1: 2×2 eigenvalues, eigenvectors and diagonalisation (NAT)**
*A = [[4, 1], [2, 3]].*

- tr = 7 and det = 12 − 2 = 10, so λ² − 7λ + 10 = 0 and **λ = 5, 2.**
- **λ = 5:** A − 5I = [[−1, 1], [2, −2]] → −x + y = 0 → **v₁ = (1, 1).**
- **λ = 2:** A − 2I = [[2, 1], [2, 1]] → 2x + y = 0 → **v₂ = (1, −2).**
- **Diagonalisation:** P = [[1, 1], [1, −2]] and D = diag(5, 2), so A = PDP⁻¹.
- Check: A·(1, 1) = (5, 5) = 5(1, 1). ✓
- A is not symmetric, so v₁ and v₂ aren't orthogonal (their dot product is −1).

**Example 2: AM vs GM (MSQ)**
*Is B = [[2, 1, 0], [0, 2, 0], [0, 0, 3]] diagonalisable?*

- B is triangular, so the eigenvalues are **2, 2, 3.** λ = 2 has AM = 2.
- B − 2I = [[0, 1, 0], [0, 0, 0], [0, 0, 1]] has rank 2, so GM = 3 − 2 = **1.**
- GM < AM for λ = 2, so B is **not diagonalisable.**
- Swap the 1 above the diagonal for a 0 and it becomes diagonalisable.

**Example 3: eigenvalues of a matrix polynomial (NAT)**
*A is 3×3 with eigenvalues 1, 2, 3. Find the eigenvalues, trace and determinant of B = A² + 2A + I. Also find det(A⁻¹).*

- B = (A + I)², so its eigenvalues are (λ + 1)²: **4, 9, 16.**
- tr B = 4 + 9 + 16 = **29.**
- det B = 4·9·16 = **576.**
- det(A⁻¹) = 1/(1·2·3) = **1/6.**
- You never need A itself.

**Example 4: projection onto a vector (NAT)**
*Project b = (3, 0, 3) onto a = (1, 2, 2). Then give the projection matrix's rank, trace and eigenvalues.*

- aᵀb = 3 + 0 + 6 = 9 and aᵀa = 1 + 4 + 4 = 9.
- proj = (9/9)·a = **(1, 2, 2).**
- Error: e = b − proj = (2, −2, 1). Check: e·a = 2 − 4 + 2 = 0, so the error is ⊥ a. ✓
- P = aaᵀ/9 has **rank 1, trace 1, eigenvalues 1, 0, 0.**
- I − P projects onto the plane ⊥ a. It has rank 2.

**Example 5: definiteness (MCQ)**
*Classify Q₁ = 2x² + 2xy + 2y² and Q₂ = x² + 4xy + y².*

- **Q₁:**
  - A = [[2, 1], [1, 2]].
  - Leading minors: 2 > 0 and 4 − 1 = 3 > 0, so it's **positive definite.**
  - Eigenvalues 3 and 1, both positive. ✓
- **Q₂:**
  - A = [[1, 2], [2, 1]]. The 4xy term gives 2 on each off-diagonal.
  - Minors: 1 > 0 but 1 − 4 = −3 < 0, so it's **indefinite.**
  - Eigenvalues 3 and −1. For example, x = 1, y = −1 gives Q₂ = 1 − 4 + 1 = −2 < 0.

**Example 6: SVD of a 2×2 by hand, plus the rank-1 approximation (NAT)**
*A = [[3, 0], [4, 5]].*

- **AᵀA** = [[3, 4], [0, 5]]·[[3, 0], [4, 5]] = **[[25, 20], [20, 25]].**
  - Its eigenvalues are 25 ± 20 = **45, 5.**
- **Singular values:** σ₁ = √45 = 3√5 ≈ **6.708**, and σ₂ = √5 ≈ **2.236.**
- **Checks:**
  - σ₁σ₂ = √225 = 15 = |det A|. ✓
  - σ₁² + σ₂² = 50 = 9 + 0 + 16 + 25 = ‖A‖_F². ✓
- **V** (eigenvectors of AᵀA): v₁ = (1, 1)/√2 and v₂ = (1, −1)/√2.
- **U** from uᵢ = Avᵢ/σᵢ:
  - Av₁ = (3, 9)/√2, so u₁ = (1, 3)/√10.
  - Av₂ = (3, −1)/√2, so u₂ = (3, −1)/√10.
- **Rank-1 approximation:**
  - A₁ = σ₁u₁v₁ᵀ = 3√5 · (1, 3)/√10 · (1, 1)ᵀ/√2 = **[[1.5, 1.5], [4.5, 4.5]].**
  - The error ‖A − A₁‖₂ = σ₂ = √5 ≈ 2.236.
- ‖A‖₂ = σ₁ ≈ 6.708. Rank = 2, since both σ are nonzero.
- (Software may flip the signs of u and v, but that doesn't change anything.)

---

## 4. Common traps

1. **Eigenvalues of A + B ≠ eig(A) + eig(B)** and **eig(AB) ≠ eig(A)·eig(B)** in general. The shortcuts only hold for **functions of one matrix**, like p(A), A⁻¹ or Aᵏ.
2. **Row operations change the eigenvalues.** Never row-reduce A to find them (unlike rank). Only the trace, determinant and triangular shortcuts are safe.
3. **Repeated eigenvalue ⇒ check GM.** Repeated doesn't automatically mean non-diagonalisable (I is fine). You have to compute rank(A − λI).
4. **Eigenvectors aren't unique.** Any nonzero multiple works, so don't mark an answer wrong for scaling.
5. **Quadratic form off-diagonals take HALF the cross-term coefficient.** 4xy → 2, not 4.
6. **Sylvester's leading-minor test checks PD only.** For PSD you need all principal minors ≥ 0, or use eigenvalues.
7. **Singular values ≠ eigenvalues,** except for symmetric PSD matrices. σᵢ = √(eig(AᵀA)), and they're always ≥ 0.
8. **An idempotent matrix's rank = its trace.** This turns many "find the rank of P" questions into one addition.
9. **Orthogonal ⇒ |λ| = 1, not λ = ±1.** A rotation's eigenvalues can be complex. Its singular values are all 1.
10. **The projection formula A(AᵀA)⁻¹Aᵀ needs independent columns.** Otherwise AᵀA isn't invertible.
11. **The zero vector is never an eigenvector,** but **0 can be an eigenvalue** (exactly when A is singular).

---

## 5. Cheat card

**Eigenvalues**
- det(A − λI) = 0. For a 2×2: λ² − (tr)λ + det = 0.
- Σλ = trace, ∏λ = det. Triangular → the diagonal entries.
- Same eigenvector, new eigenvalue: Aᵏ → λᵏ, A⁻¹ → 1/λ, A + kI → λ + k, p(A) → p(λ).
- Aᵀ has the same eigenvalues. Similar matrices share eigenvalues. AB and BA share the nonzero ones.
- Rank-1 uvᵀ → eigenvalues vᵀu and 0s. The all-1s n×n matrix → n and 0s.
- Cayley–Hamilton: p_A(A) = 0.

**Diagonalisation**
- 1 ≤ GM ≤ AM. Diagonalisable ⇔ GM = AM for every λ. Distinct eigenvalues ⇒ diagonalisable.
- Symmetric ⇒ real λ, orthogonal eigenvectors, A = QΛQᵀ.

**Special matrices**
- Orthogonal: Q⁻¹ = Qᵀ, det ±1, |λ| = 1, preserves lengths.
- Idempotent: P² = P, λ ∈ {0, 1}, rank = trace, I − P is idempotent.
- Projection onto a: aaᵀ/aᵀa. Onto C(A): A(AᵀA)⁻¹Aᵀ. Symmetric and idempotent.
- Nilpotent: all λ = 0. Involutory: λ = ±1. Skew: λ purely imaginary or 0.
- Block diagonal or triangular: det = det A·det D, eigenvalues = union.

**Quadratic forms**
- ax² + 2bxy + cy² ↔ [[a, b], [b, c]].
- PD ⇔ all λ > 0 ⇔ all leading minors > 0. PSD ⇔ all λ ≥ 0. Indefinite ⇔ mixed signs.
- AᵀA is PSD. max xᵀAx on ‖x‖ = 1 is λ_max.

**SVD**
- A = UΣVᵀ. σᵢ = √eig(AᵀA). V = eigenvectors of AᵀA, U = eigenvectors of AAᵀ, uᵢ = Avᵢ/σᵢ.
- rank = number of σᵢ > 0. ‖A‖₂ = σ₁. ‖A‖_F² = Σσᵢ². |det| = ∏σᵢ.
- Best rank-k approximation: top-k σ terms, with error σₖ₊₁ in the 2-norm.
- PCA = right singular vectors of the centred data, with variance σᵢ²/(n−1).

---

## 6. Practice

**Q1 (NAT, 1 decimal).** A = [[2, 0, 0], [1, 3, 0], [4, 5, 6]]. What is the largest eigenvalue of A⁻¹?

**Q2 (NAT).** A 3×3 matrix A has eigenvalues 1, 2 and 3. Find trace(A³).

**Q3 (MSQ).** P is an n×n matrix with P² = P. Which of these are ALWAYS true?
(a) Every eigenvalue of P is 0 or 1
(b) rank(P) = trace(P)
(c) I − P is also idempotent
(d) P is invertible

**Q4 (MCQ).** The quadratic form Q(x, y) = 2x² − 2xy + 2y² is:
(a) positive definite
(b) positive semidefinite but not definite
(c) negative definite
(d) indefinite

**Q5 (NAT).** Find the largest singular value of A = [[1, 2], [2, 4]].

**Q6 (NAT).** A = [[5, 4], [1, 2]]. Find the largest eigenvalue of A + 3I.

### Recall: Day 4

**R1.** A 4 × 6 matrix has rank 4. What is its nullity?

**R2.** A is 2×2 with det A = 2. What is det(3A)?

**R3.** How many solutions does the system x + y = 1, x + y = 2 have?

---

**Next, Day 6:** calculus and optimisation. Limits, continuity and differentiability, Taylor series, maxima and minima, and gradient descent.

---

<details>
<summary><b>Answers</b> (click to open)</summary>

**Q1: 0.5.**
- A is triangular, so its eigenvalues are 2, 3 and 6.
- A⁻¹ has eigenvalues 1/2, 1/3 and 1/6. The largest is 1/2.

**Q2: 36.** A³ has eigenvalues 1, 8 and 27, so the trace is 1 + 8 + 27 = 36.

**Q3: (a), (b), (c).**
- (a): λ² = λ ⇒ λ ∈ {0, 1}.
- (b) is the standard rank = trace fact for idempotents.
- (c): (I − P)² = I − 2P + P² = I − P.
- (d) is false: only I is an invertible idempotent. For example, the zero matrix is idempotent.

**Q4: (a).**
- The matrix is [[2, −1], [−1, 2]].
- Leading minors: 2 > 0 and 4 − 1 = 3 > 0. Eigenvalues 1 and 3.

**Q5: 5.**
- A is symmetric with rank 1, so its eigenvalues are its trace and 0: 5 and 0.
- It's also PSD, so σ = 5. Check: AᵀA has eigenvalues 25 and 0, and √25 = 5.

**Q6: 9.**
- λ² − 7λ + 6 = 0, so λ = 6 and 1.
- Adding 3I adds 3 to each eigenvalue: 9 and 4.

**Recall**
- **R1:** 6 − 4 = 2 (count the columns).
- **R2:** 3² × 2 = 18.
- **R3:** none. The equations contradict each other: rank 1 < augmented rank 2.

</details>
