# Master Study Notes: Mathematical Foundations for Computer Applications
## Course Code: 22MCA11 / MMC102 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Set Theory, Relations, Inclusion-Exclusion Principle & Equivalence Relations.
* **Module 2**: Matrix Algebra, Systems of Linear Equations, Gauss Elimination, Eigenvalues & Eigenvectors, Cayley-Hamilton Theorem.
* **Module 3**: Graph Theory, Handshaking Lemma, Isomorphism, Eulerian & Hamiltonian Graphs, Trees, Spanning Trees, Prim's & Kruskal's MST Algorithms.
* **Module 4**: Probability Theory, Axioms, Conditional Probability, Bayes' Theorem, Random Variables (Discrete & Continuous), PMF/PDF, Probability Distributions (Binomial, Poisson, Normal).
* **Module 5**: Statistical Inference & Hypothesis Testing, Type I & Type II Errors, Large Sample Z-Tests, Small Sample Student's t-Tests, Chi-Square ($\chi^2$) Goodness-of-Fit & Independence Tests.

---

# MODULE 1: SET THEORY & DISCRETE RELATIONS

## 1.1 Fundamental Definitions
* **Set**: An unordered collection of distinct, well-defined objects.
* **Subset ($A \subseteq B$)**: $\forall x (x \in A \implies x \in B)$.
* **Power Set ($\mathcal{P}(A)$)**: The set of all subsets of $A$. If $|A| = n$, then $|\mathcal{P}(A)| = 2^n$.
* **Cartesian Product ($A \times B$)**: $\{(a, b) \mid a \in A \land b \in B\}$. $|A \times B| = |A| \cdot |B|$.

## 1.2 Principle of Inclusion-Exclusion (PIE)
### Two Sets:
$$|A \cup B| = |A| + |B| - |A \cap B|$$

### Three Sets (Proof frequently asked for 10 Marks):
$$|A \cup B \cup C| = |A| + |B| + |C| - (|A \cap B| + |B \cap C| + |A \cap C|) + |A \cap B \cap C|$$

#### Step-by-Step Proof:
1. Let $D = B \cup C$. Then $A \cup B \cup C = A \cup D$.
2. By two-set inclusion-exclusion:
   $$|A \cup D| = |A| + |D| - |A \cap D|$$
3. Substitute $D = B \cup C$:
   $$|A \cup (B \cup C)| = |A| + (|B| + |C| - |B \cap C|) - |A \cap (B \cup C)|$$
4. By distributive law of sets: $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$.
5. Apply the two-set union rule to $|(A \cap B) \cup (A \cap C)|$:
   $$|(A \cap B) \cup (A \cap C)| = |A \cap B| + |A \cap C| - |(A \cap B) \cap (A \cap C)|$$
   Since $(A \cap B) \cap (A \cap C) = A \cap B \cap C$:
   $$|(A \cap B) \cup (A \cap C)| = |A \cap B| + |A \cap C| - |A \cap B \cap C|$$
6. Substitute back into equation (3):
   $$|A \cup B \cup C| = |A| + |B| + |C| - |B \cap C| - \big(|A \cap B| + |A \cap C| - |A \cap B \cap C|\big)$$
   $$|A \cup B \cup C| = |A| + |B| + |C| - |A \cap B| - |B \cap C| - |A \cap C| + |A \cap B \cap C| \quad \blacksquare$$

---

## 1.3 Relations & Equivalence Classes
A binary relation $R$ from set $A$ to set $B$ is a subset of $A \times B$.

### Properties of a Relation $R$ on set $A$:
1. **Reflexive**: $\forall a \in A, (a, a) \in R$.
2. **Symmetric**: $\forall a, b \in A, (a, b) \in R \implies (b, a) \in R$.
3. **Transitive**: $\forall a, b, c \in A, ((a, b) \in R \land (b, c) \in R) \implies (a, c) \in R$.
4. **Anti-Symmetric**: $\forall a, b \in A, ((a, b) \in R \land (b, a) \in R) \implies a = b$.

* **Equivalence Relation**: A relation that is **Reflexive, Symmetric, and Transitive**.
* **Partial Order Relation (POSET)**: A relation that is **Reflexive, Anti-Symmetric, and Transitive**.

---

# MODULE 2: MATRIX ALGEBRA & SYSTEMS OF LINEAR EQUATIONS

## 2.1 Rank of a Matrix
The rank of a matrix $A$, denoted $\rho(A)$, is the maximum number of linearly independent row vectors (or the order of the highest non-zero minor). It is obtained by reducing $A$ to **Row Echelon Form** using elementary row operations ($R_i \leftrightarrow R_j$, $R_i \leftarrow k R_i$, $R_i \leftarrow R_i + k R_j$).

## 2.2 System of Linear Equations ($AX = B$)
Let $[A \mid B]$ be the augmented matrix.
1. **Consistent with Unique Solution**: $\rho(A) = \rho([A \mid B]) = n$ (where $n$ is the number of unknowns).
2. **Consistent with Infinitely Many Solutions**: $\rho(A) = \rho([A \mid B]) = r < n$. ($n - r$ free variables).
3. **Inconsistent (No Solution)**: $\rho(A) \neq \rho([A \mid B])$ (specifically $\rho(A) < \rho([A \mid B])$).

## 2.3 Gauss Elimination Method
1. Form augmented matrix $[A \mid B]$.
2. Apply row operations to transform $A$ into Upper Triangular Form.
3. Solve for unknowns using **Back Substitution**.

## 2.4 Eigenvalues and Eigenvectors
For an $n \times n$ square matrix $A$:
$$A X = \lambda X \implies (A - \lambda I) X = 0$$
* **Characteristic Equation**: $\det(A - \lambda I) = 0$.
* The roots of this polynomial are the **Eigenvalues ($\lambda_i$)**.
* For each $\lambda_i$, the non-zero vector $X_i$ satisfying $(A - \lambda_i I) X_i = 0$ is the **Eigenvector**.

### Important Properties of Eigenvalues:
1. $\sum \lambda_i = \text{Trace}(A)$ (Sum of main diagonal elements).
2. $\prod \lambda_i = \det(A)$.
3. If $\lambda$ is an eigenvalue of $A$, then $\lambda^k$ is an eigenvalue of $A^k$, and $\frac{1}{\lambda}$ is an eigenvalue of $A^{-1}$ (if $\det(A) \neq 0$).

## 2.5 Cayley-Hamilton Theorem
* **Statement**: *Every square matrix satisfies its own characteristic equation.*
* If the characteristic polynomial is $P(\lambda) = \lambda^n + c_{n-1} \lambda^{n-1} + \dots + c_0 = 0$, then:
  $$A^n + c_{n-1} A^{n-1} + \dots + c_0 I = 0$$
* **Applications**:
  1. Calculating high matrix powers: $A^4, A^8$.
  2. Finding inverse matrix: Multiply equation by $A^{-1}$:
     $$A^{-1} = -\frac{1}{c_0} (A^{n-1} + c_{n-1} A^{n-2} + \dots + c_1 I)$$

---

# MODULE 3: GRAPH THEORY & COMBINATORICS

## 3.1 Graph Terminology
* $G = (V, E)$, where $V$ is the vertex set, $E$ is the edge set.
* **Degree of a Vertex ($d(v)$)**: Number of edges incident on $v$ (self-loop counts twice).

### The Handshaking Lemma (Fundamental Theorem of Graph Theory):
$$\sum_{v \in V} \deg(v) = 2 |E|$$
* **Corollary**: In every undirected graph, the number of vertices with odd degree is always **even**.

## 3.2 Eulerian and Hamiltonian Graphs
* **Eulerian Path**: A trail in a finite graph that visits every edge exactly once.
* **Eulerian Circuit**: An Eulerian trail that starts and ends at the same vertex.
  * *Euler's Theorem*: A connected graph has an Euler circuit if and only if **every vertex has an even degree**.
* **Hamiltonian Cycle**: A closed loop that visits **every vertex exactly once** (except the start/end vertex). (NP-Complete problem).

## 3.3 Trees and Spanning Trees
* **Tree**: A connected acyclic undirected graph.
  * Properties: A tree with $n$ vertices has exactly $n - 1$ edges. Adding any edge creates a unique cycle.
* **Spanning Tree**: A subgraph that is a tree and includes all vertices of $G$.
* **Minimum Spanning Tree (MST)**: In a weighted graph, a spanning tree with the minimum possible total edge weight.

### Prim's vs Kruskal's Algorithms:
| Feature | Prim's Algorithm | Kruskal's Algorithm |
| :--- | :--- | :--- |
| **Approach** | Vertex-based growing tree | Edge-based greedy sorting |
| **Data Structure** | Priority Queue / Min-Heap | Disjoint Set Union (Union-Find) |
| **Time Complexity** | $O(E \log V)$ with adjacency list | $O(E \log E)$ or $O(E \log V)$ |
| **Best Suited For** | Dense graphs ($E \approx V^2$) | Sparse graphs ($E \ll V^2$) |

---

# MODULE 4: PROBABILITY THEORY & DISTRIBUTIONS

## 4.1 Conditional Probability & Multiplication Rule
$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}, \quad \text{provided } P(B) > 0$$
$$P(A \cap B) = P(B) \cdot P(A \mid B) = P(A) \cdot P(B \mid A)$$

## 4.2 Law of Total Probability
Let $B_1, B_2, \dots, B_k$ be a partition of sample space $S$. For any event $A$:
$$P(A) = \sum_{i=1}^k P(B_i) \cdot P(A \mid B_i)$$

## 4.3 Bayes' Theorem (Core VTU Exam Favorite)
$$P(B_j \mid A) = \frac{P(B_j) \cdot P(A \mid B_j)}{\sum_{i=1}^k P(B_i) \cdot P(A \mid B_i)}$$

## 4.4 Probability Distributions
### 1. Binomial Distribution $B(n, p)$:
* $P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}, \quad k = 0, 1, 2, \dots, n$
* $\text{Mean } \mu = n p$
* $\text{Variance } \sigma^2 = n p q = n p (1-p)$

### 2. Poisson Distribution $P(\lambda)$:
* Used for rare events where $n$ is large and $p$ is small, $\lambda = n p$.
* $P(X = k) = \frac{e^{-\lambda} \lambda^k}{k!}, \quad k = 0, 1, 2, \dots$
* $\text{Mean } \mu = \lambda$
* $\text{Variance } \sigma^2 = \lambda$ (Mean equals Variance).

### 3. Normal Distribution $N(\mu, \sigma^2)$:
* Continuous bell-shaped curve: $f(x) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{(x - \mu)^2}{2\sigma^2}}$.
* Standard Normal Variable $Z = \frac{X - \mu}{\sigma} \sim N(0, 1)$.

---

# MODULE 5: STATISTICAL INFERENCE & HYPOTHESIS TESTING

## 5.1 Terminology
* **Null Hypothesis ($H_0$)**: Status quo or claim of no difference (e.g., $\mu = \mu_0$).
* **Alternative Hypothesis ($H_1$)**: Contradiction to $H_0$ (Two-tailed $\mu \neq \mu_0$, Right-tailed $\mu > \mu_0$, Left-tailed $\mu < \mu_0$).
* **Type I Error ($\alpha$)**: Rejecting $H_0$ when $H_0$ is TRUE (Producer's risk / Level of Significance).
* **Type II Error ($\beta$)**: Accepting $H_0$ when $H_0$ is FALSE (Consumer's risk).

## 5.2 Test Statistics
### 1. Large Sample Z-Test ($n \ge 30$):
* Single Mean: $Z = \frac{\bar{X} - \mu}{\sigma / \sqrt{n}}$
* Two Means: $Z = \frac{\bar{X}_1 - \bar{X}_2}{\sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}}}$
* Critical Values for $Z$ (Two-tailed):
  * $5\%$ Level ($\alpha = 0.05$): Critical $Z = 1.96$.
  * $1\%$ Level ($\alpha = 0.01$): Critical $Z = 2.58$.

### 2. Small Sample Student's t-Test ($n < 30$):
* $t = \frac{\bar{X} - \mu}{S / \sqrt{n}}$ with degrees of freedom $\nu = n - 1$, where $S = \sqrt{\frac{\sum (X_i - \bar{X})^2}{n - 1}}$.

### 3. Chi-Square ($\chi^2$) Test for Independence of Attributes:
$$\chi^2 = \sum \frac{(O - E)^2}{E}$$
Where $O$ is Observed Frequency, and $E$ is Expected Frequency:
$$E = \frac{\text{Row Total} \times \text{Column Total}}{\text{Grand Total}}$$
Degrees of freedom: $\nu = (r - 1)(c - 1)$.

---

# 🎯 Model Exam Questions with Step-by-Step Solutions

### Q1. [Module 1 - 10 Marks]
**In a survey of 200 MCA graduates: 120 know Python, 100 know Java, 80 know SQL. 50 know Python & Java, 40 know Java & SQL, 35 know Python & SQL, and 20 know all three languages.**
1. How many students know at least one language?
2. How many students know none of these languages?
3. How many know strictly Python only?

**Solution:**
1. Let $P, J, S$ represent students knowing Python, Java, and SQL.
   $|P| = 120, |J| = 100, |S| = 80$
   $|P \cap J| = 50, |J \cap S| = 40, |P \cap S| = 35$
   $|P \cap J \cap S| = 20$

   By Principle of Inclusion-Exclusion:
   $$|P \cup J \cup S| = 120 + 100 + 80 - (50 + 40 + 35) + 20$$
   $$|P \cup J \cup S| = 300 - 125 + 20 = 195 \text{ students}.$$

2. Students knowing none:
   $$\text{Total} - |P \cup J \cup S| = 200 - 195 = 5 \text{ students}.$$

3. Students knowing Python only:
   $$\text{Only } P = |P| - |P \cap J| - |P \cap S| + |P \cap J \cap S|$$
   $$\text{Only } P = 120 - 50 - 35 + 20 = 55 \text{ students}.$$

---

### Q2. [Module 2 - 10 Marks]
**Find the Eigenvalues and corresponding Eigenvectors for the matrix:**
$$A = \begin{bmatrix} 4 & 2 \\ 1 & 3 \end{bmatrix}$$

**Solution:**
1. **Characteristic Equation**: $\det(A - \lambda I) = 0$.
   $$\begin{vmatrix} 4 - \lambda & 2 \\ 1 & 3 - \lambda \end{vmatrix} = 0$$
   $$(4 - \lambda)(3 - \lambda) - (2)(1) = 0$$
   $$\lambda^2 - 7\lambda + 12 - 2 = 0 \implies \lambda^2 - 7\lambda + 10 = 0$$
   $$(\lambda - 5)(\lambda - 2) = 0 \implies \lambda_1 = 5, \; \lambda_2 = 2$$

2. **Eigenvector for $\lambda_1 = 5$**:
   $$(A - 5I)X = 0 \implies \begin{bmatrix} -1 & 2 \\ 1 & -2 \end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}$$
   $-x_1 + 2x_2 = 0 \implies x_1 = 2x_2$.
   Let $x_2 = 1 \implies x_1 = 2$.
   $$X_1 = \begin{bmatrix} 2 \\ 1 \end{bmatrix}$$

3. **Eigenvector for $\lambda_2 = 2$**:
   $$(A - 2I)X = 0 \implies \begin{bmatrix} 2 & 2 \\ 1 & 1 \end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}$$
   $2x_1 + 2x_2 = 0 \implies x_1 = -x_2$.
   Let $x_2 = 1 \implies x_1 = -1$.
   $$X_2 = \begin{bmatrix} -1 \\ 1 \end{bmatrix}$$

---

### Q3. [Module 4 - 10 Marks]
**In a software lab, Machine A produces 60% of code modules and Machine B produces 40%. 5% of modules from Machine A have security vulnerabilities, while 2% of modules from Machine B have vulnerabilities. A randomly inspected module is found to have a vulnerability. What is the probability that it was produced by Machine A?**

**Solution:**
* Let $E_1$: Module from Machine A $\implies P(E_1) = 0.60$.
* Let $E_2$: Module from Machine B $\implies P(E_2) = 0.40$.
* Let $V$: Vulnerable module.
  * $P(V \mid E_1) = 0.05$
  * $P(V \mid E_2) = 0.02$

By Bayes' Theorem:
$$P(E_1 \mid V) = \frac{P(E_1) \cdot P(V \mid E_1)}{P(E_1) \cdot P(V \mid E_1) + P(E_2) \cdot P(V \mid E_2)}$$
$$P(E_1 \mid V) = \frac{0.60 \times 0.05}{(0.60 \times 0.05) + (0.40 \times 0.02)}$$
$$P(E_1 \mid V) = \frac{0.030}{0.030 + 0.008} = \frac{0.030}{0.038} = \frac{30}{38} \approx \mathbf{0.7895} \; (78.95\%)$$
