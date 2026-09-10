# 📐 QB 04: Mathematical Foundations for MCA — Question Bank with Answer Keys

> **Course:** Mathematical Foundations for Computer Applications (Discrete Math, Linear Algebra & Probability)  
> **Target:** VTU MCA Semester 1 (Tailored for students who want intuitive, step-by-step clarity)  
> **Scheme:** VTU 2022 / 2024 Scheme  
> **Contents:** Logic Truth Tables, Equivalence Relations, Graph Handshaking, Matrix Eigenvalues, Bayes' Theorem, 15 MCQs with Explanations

---

## 📑 Syllabus Outline (VTU 5 Modules)
- **Module 1:** Propositional Logic, Truth Tables, Logical Connectives, Tautology, De Morgan's Laws, Predicates, and Quantifiers ($\forall, \exists$).
- **Module 2:** Set Theory, Relations (Reflexive, Symmetric, Transitive, Equivalence Relations, Equivalence Classes), Posets & Hasse Diagrams, Functions (One-to-one, Onto, Invertible).
- **Module 3:** Graph Theory: Basic Terminology, Handshaking Theorem, Regular/Bipartite Graphs, Eulerian & Hamiltonian Graphs, Adjacency Matrices, Planar Graphs & Euler's Formula.
- **Module 4:** Linear Algebra: Matrix Operations, Determinants, Matrix Inverse, Systems of Linear Equations (Cramer's Rule, Gaussian Elimination), Eigenvalues & Eigenvectors.
- **Module 5:** Probability & Statistics: Axioms of Probability, Conditional Probability, Bayes' Theorem, Discrete Random Variables, Expectation (Mean) & Variance.

---

## 🎯 SECTION 1: High-Yield MCQs with Answer Key

### Q1. A compound proposition that is always TRUE regardless of the truth values of its individual variables is called a:
- A) Contradiction
- B) Contingency
- C) Tautology
- D) Fallacy  
**Answer: C**  
**Explanation:** A tautology evaluates to True ($T$) across every single row of its truth table (e.g., $p \lor \neg p$).

---

### Q2. According to De Morgan's Law, what is the logical negation of $(p \land q)$?
- A) $\neg p \land \neg q$
- B) $\neg p \lor \neg q$
- C) $p \lor q$
- D) $\neg p \to q$  
**Answer: B**  
**Explanation:** $\neg(p \land q) \equiv \neg p \lor \neg q$. Negating an "AND" statement flips it into an "OR" of the negated variables.

---

### Q3. A relation $R$ on set $A$ is an "Equivalence Relation" if it is:
- A) Reflexive, Antisymmetric, Transitive
- B) Reflexive, Symmetric, Transitive
- C) Irreflexive, Symmetric, Transitive
- D) Symmetric and Transitive only  
**Answer: B**  
**Explanation:** An equivalence relation must satisfy three properties: (1) Reflexive, (2) Symmetric, and (3) Transitive. (If antisymmetric instead of symmetric, it is a Partial Order / Poset).

---

### Q4. The Handshaking Lemma states that in any undirected graph $G = (V, E)$:
- A) $\sum_{v \in V} \text{deg}(v) = |E|$
- B) $\sum_{v \in V} \text{deg}(v) = 2|E|$
- C) $\sum_{v \in V} \text{deg}(v) = |V| \times |E|$
- D) Total odd vertices is always odd  
**Answer: B**  
**Explanation:** Every edge contributes 1 to the degree of each of its two endpoints; thus, the sum of all vertex degrees is always twice the number of edges ($2|E|$).

---

### Q5. If an undirected graph has 10 edges, what is the sum of the degrees of all vertices?
- A) 10
- B) 15
- C) 20
- D) 25  
**Answer: C**  
**Explanation:** By Handshaking Lemma: $\sum \text{deg}(v) = 2 \times |E| = 2 \times 10 = 20$.

---

### Q6. For a connected planar graph with $V$ vertices, $E$ edges, and $F$ faces/regions, Euler's formula states:
- A) $V - E + F = 2$
- B) $V + E - F = 2$
- C) $V - E - F = 0$
- D) $V \times F = E$  
**Answer: A**  
**Explanation:** Euler's Planar Formula: $V - E + F = 2$ for any connected planar graph.

---

### Q7. If the determinant of a square matrix $A$ is zero ($\det(A) = 0$), the matrix is called:
- A) Orthogonal
- B) Non-singular
- C) Singular (and has no inverse)
- D) Symmetric  
**Answer: C**  
**Explanation:** A matrix with determinant 0 is singular and cannot be inverted because $A^{-1} = \frac{1}{\det(A)} \text{adj}(A)$, which would require division by zero.

---

### Q8. The eigenvalues ($\lambda$) of a square matrix $A$ are roots of which characteristic equation?
- A) $\det(A) = 0$
- B) $\det(A - \lambda I) = 0$
- C) $A \cdot \lambda = I$
- D) $\text{Tr}(A) - \lambda = 0$  
**Answer: B**  
**Explanation:** Non-trivial solutions to $A x = \lambda x \implies (A - \lambda I)x = 0$ require that the determinant $|A - \lambda I| = 0$.

---

### Q9. If events $A$ and $B$ are mutually exclusive, what is $P(A \cap B)$?
- A) 1
- B) $P(A) \times P(B)$
- C) 0
- D) $P(A) + P(B)$  
**Answer: C**  
**Explanation:** Mutually exclusive events cannot happen at the same time; their intersection is an empty set $\emptyset$, so $P(A \cap B) = 0$.

---

### Q10. Bayes' Theorem computes the posterior probability $P(A|B)$ as:
- A) $\frac{P(B|A) P(A)}{P(B)}$
- B) $\frac{P(A \cap B)}{P(A)}$
- C) $P(A) + P(B|A)$
- D) $P(A) \times P(B)$  
**Answer: A**  
**Explanation:** Bayes' formula is $P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}$.

---

## 🏛️ SECTION 2: Step-by-Step Concept Questions (4–6 Marks)

### Q11. Define Equivalence Relation. Prove that congruence modulo $n$ on integers ($a \equiv b \pmod n$) is an equivalence relation.
**Answer:**

**Definition:** A relation $R$ on a set $A$ is an **equivalence relation** if it satisfies:
1. **Reflexive:** $\forall a \in A, \; (a, a) \in R$.
2. **Symmetric:** If $(a, b) \in R \implies (b, a) \in R$.
3. **Transitive:** If $(a, b) \in R$ and $(b, c) \in R \implies (a, c) \in R$.

**Proof for Congruence Modulo $n$:**  
Relation: $a \equiv b \pmod n \iff (a - b)$ is divisible by $n$ (i.e., $a - b = k \cdot n$ for some integer $k$).

1. **Reflexive:**
   - $a - a = 0 = 0 \cdot n$.
   - Since $0$ is divisible by $n$, $a \equiv a \pmod n$. $\implies$ **Reflexive**.
2. **Symmetric:**
   - Assume $a \equiv b \pmod n$. Then $a - b = k \cdot n$.
   - Multiply both sides by $-1$: $b - a = (-k) \cdot n$.
   - Since $-k$ is also an integer, $(b - a)$ is divisible by $n$.
   - Hence, $b \equiv a \pmod n$. $\implies$ **Symmetric**.
3. **Transitive:**
   - Assume $a \equiv b \pmod n$ and $b \equiv c \pmod n$.
   - Then $a - b = k_1 \cdot n$ and $b - c = k_2 \cdot n$.
   - Adding both equations:
     $$(a - b) + (b - c) = a - c = (k_1 + k_2) \cdot n$$
   - Since $(k_1 + k_2)$ is an integer, $(a - c)$ is divisible by $n$.
   - Thus, $a \equiv c \pmod n$. $\implies$ **Transitive**.

**Conclusion:** Since the relation is reflexive, symmetric, and transitive, it is an **Equivalence Relation**. $\blacksquare$

---

### Q12. State the Handshaking Theorem. Prove that in any graph, the number of vertices with odd degree is always even.
**Answer:**

**Handshaking Theorem:**  
In any undirected graph $G = (V, E)$:
$$\sum_{v \in V} \text{deg}(v) = 2|E|$$

**Proof:**
Divide the vertex set $V$ into two disjoint subsets:
- $V_1$: Set of vertices with **even** degree.
- $V_2$: Set of vertices with **odd** degree.

Then:
$$\sum_{v \in V} \text{deg}(v) = \sum_{v \in V_1} \text{deg}(v) + \sum_{v \in V_2} \text{deg}(v) = 2|E|$$

1. Notice that $2|E|$ is always an **even number**.
2. The sum of degrees of vertices in $V_1$ ($\sum_{v \in V_1} \text{deg}(v)$) is a sum of even numbers, which is always **even**.
3. Subtracting the even sum:
   $$\sum_{v \in V_2} \text{deg}(v) = 2|E| - \sum_{v \in V_1} \text{deg}(v) = \text{Even} - \text{Even} = \mathbf{Even}$$
4. For the sum of odd numbers to be **even**, there must be an **even number of terms** in the sum!
5. Therefore, $|V_2|$ (the count of vertices with odd degree) must be **even**. $\blacksquare$

---

## 🏛️ SECTION 3: VTU Model Numerical Problems (10–12 Marks)

### Q13. [VTU Model QP - Truth Table Proof]
**Construct a Truth Table to prove that the following proposition is a Tautology:**  
$$[(p \to q) \land (q \to r)] \to (p \to r)$$  
*(Law of Hypothetical Syllogism)*

**Answer:**

Let us evaluate step-by-step for all $2^3 = 8$ truth assignments of $(p, q, r)$:
- Recall: An implication $A \to B$ is False **only when $A = T$ and $B = F$**; in all other cases, it is True ($T$).

| $p$ | $q$ | $r$ | $p \to q$ | $q \to r$ | Premise: $(p \to q) \land (q \to r)$ | Conclusion: $(p \to r)$ | Full Expression: $\text{Premise} \to \text{Conclusion}$ |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| T | T | T | T | T | **T** | **T** | **T** |
| T | T | F | T | F | **F** | **F** | **T** |
| T | F | T | F | T | **F** | **T** | **T** |
| T | F | F | F | T | **F** | **F** | **T** |
| F | T | T | T | T | **T** | **T** | **T** |
| F | T | F | T | F | **F** | **T** | **T** |
| F | F | T | T | T | **T** | **T** | **T** |
| F | F | F | T | T | **T** | **T** | **T** |

**Conclusion:**  
Every single entry in the final column is **True ($T$)**.  
Therefore, the proposition is a **Tautology**. $\blacksquare$

---

### Q14. [VTU Model QP - Linear Algebra: Eigenvalues & Eigenvectors]
**Find the Eigenvalues and corresponding Eigenvectors for the matrix:**  
$$A = \begin{pmatrix} 4 & 1 \\ 2 & 3 \end{pmatrix}$$

**Answer:**

#### Step 1: Characteristic Equation
$$\det(A - \lambda I) = 0$$

$$\begin{vmatrix} 4 - \lambda & 1 \\ 2 & 3 - \lambda \end{vmatrix} = 0$$

$$(4 - \lambda)(3 - \lambda) - (1 \times 2) = 0$$
$$12 - 4\lambda - 3\lambda + \lambda^2 - 2 = 0$$
$$\lambda^2 - 7\lambda + 10 = 0$$

Factorizing the quadratic equation:
$$(\lambda - 5)(\lambda - 2) = 0$$
$$\mathbf{\lambda_1 = 5, \quad \lambda_2 = 2}$$
The **Eigenvalues** are **5** and **2**.

---

#### Step 2: Eigenvector for $\lambda_1 = 5$
Solve $(A - 5I)X = 0$:
$$\begin{pmatrix} 4 - 5 & 1 \\ 2 & 3 - 5 \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

$$\begin{pmatrix} -1 & 1 \\ 2 & -2 \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

From row 1: $-x_1 + x_2 = 0 \implies x_1 = x_2$.  
Let $x_2 = 1 \implies x_1 = 1$.  
**Eigenvector for $\lambda = 5$:**
$$X_1 = \begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

---

#### Step 3: Eigenvector for $\lambda_2 = 2$
Solve $(A - 2I)X = 0$:
$$\begin{pmatrix} 4 - 2 & 1 \\ 2 & 3 - 2 \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

$$\begin{pmatrix} 2 & 1 \\ 2 & 1 \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}$$

From row 1: $2x_1 + x_2 = 0 \implies x_2 = -2x_1$.  
Let $x_1 = 1 \implies x_2 = -2$.  
**Eigenvector for $\lambda = 2$:**
$$X_2 = \begin{pmatrix} 1 \\ -2 \end{pmatrix}$$

---

### Q15. [VTU Model QP - Bayes' Theorem Problem]
**In a software company, a spam filter evaluates incoming emails. It is known that 20% of all emails are spam ($S$) and 80% are legitimate/ham ($H$).**  
- **If an email is spam, the probability that it contains the word "Discount" is 90% ($P(D|S) = 0.90$).**  
- **If an email is legitimate, the probability that it contains the word "Discount" is only 5% ($P(D|H) = 0.05$).**

**If a randomly received email contains the word "Discount", what is the probability that it is actually SPAM?**

**Answer:**

#### Step 1: Identify Given Probabilities
- Prior probability of Spam: $P(S) = 0.20$
- Prior probability of Legitimate (Ham): $P(H) = 0.80$
- Likelihood of "Discount" given Spam: $P(D|S) = 0.90$
- Likelihood of "Discount" given Ham: $P(D|H) = 0.05$

#### Step 2: Total Probability of an email containing "Discount" ($P(D)$)
By the Law of Total Probability:
$$P(D) = P(D|S) \cdot P(S) + P(D|H) \cdot P(H)$$
$$P(D) = (0.90 \times 0.20) + (0.05 \times 0.80)$$
$$P(D) = 0.18 + 0.04 = \mathbf{0.22}$$

#### Step 3: Apply Bayes' Theorem
We want to find $P(S|D)$ (the probability that the email is Spam, given that it contains "Discount"):
$$P(S|D) = \frac{P(D|S) \cdot P(S)}{P(D)}$$

$$P(S|D) = \frac{0.90 \times 0.20}{0.22} = \frac{0.18}{0.22} = \frac{18}{22} = \frac{9}{11} \approx \mathbf{0.8182}$$

**Final Answer:**  
The probability that the email is spam is $\mathbf{81.82\%}$.

---

## 💡 Key Takeaway Checklist for Exam Day
- [ ] For truth tables, double-check that $p \to q$ is false **ONLY** when $p$ is True and $q$ is False.
- [ ] For graph problems, always verify using the Handshaking formula: $2 \times \text{Edges} = \text{Sum of degrees}$.
- [ ] When finding eigenvalues, confirm: $\lambda_1 + \lambda_2 = \text{Trace}(A)$ (sum of diagonal entries) and $\lambda_1 \times \lambda_2 = \det(A)$.  
  *(Check for matrix $A$: $5 + 2 = 7 = 4 + 3$, and $5 \times 2 = 10 = (12 - 2)$. Matches perfectly!).*
