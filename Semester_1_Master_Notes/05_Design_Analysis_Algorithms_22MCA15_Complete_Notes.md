# Master Study Notes: Design & Analysis of Algorithms
## Course Code: 22MCA15 / MMC203 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Algorithm Analysis, Time & Space Complexity, Asymptotic Notations ($O, \Omega, \Theta, o, \omega$), Mathematical Induction, Recurrence Relations (Substitution, Recursion Tree, Master Theorem).
* **Module 2**: Divide and Conquer Paradigm, Binary Search, Merge Sort, Quick Sort (Partitioning, Best/Worst Cases), Strassen's Matrix Multiplication ($O(n^{2.81})$).
* **Module 3**: The Greedy Technique, Fractional Knapsack, Minimum Spanning Trees (Prim's & Kruskal's Algorithms), Dijkstra's Single-Source Shortest Path, Huffman Data Compression.
* **Module 4**: Dynamic Programming Paradigm, Principle of Optimality, 0/1 Knapsack Problem (Table Trace & Backtracking), All-Pairs Shortest Path (Floyd-Warshall), Matrix Chain Multiplication, Longest Common Subsequence (LCS).
* **Module 5**: Backtracking (State Space Tree, N-Queens Problem, Sum of Subsets), Branch and Bound (Assignment Problem, TSP), Theory of NP-Completeness (P, NP, NP-Complete, NP-Hard, Cook's Theorem).

---

# MODULE 1: ASYMPTOTIC ANALYSIS & RECURRENCES

## 1.1 Formal Definitions of Asymptotic Notations

### 1. Big-O ($O$) — Asymptotic Upper Bound:
$f(n) = O(g(n))$ if and only if there exist positive constants $c > 0$ and $n_0 \ge 1$ such that:
$$0 \le f(n) \le c \cdot g(n) \quad \forall n \ge n_0$$

### 2. Big-Omega ($\Omega$) — Asymptotic Lower Bound:
$f(n) = \Omega(g(n))$ if and only if there exist positive constants $c > 0$ and $n_0 \ge 1$ such that:
$$0 \le c \cdot g(n) \le f(n) \quad \forall n \ge n_0$$

### 3. Big-Theta ($\Theta$) — Asymptotically Tight Bound:
$f(n) = \Theta(g(n))$ if and only if there exist positive constants $c_1, c_2 > 0$ and $n_0 \ge 1$ such that:
$$0 \le c_1 \cdot g(n) \le f(n) \le c_2 \cdot g(n) \quad \forall n \ge n_0$$
* **Theorem**: $f(n) = \Theta(g(n)) \iff f(n) = O(g(n)) \land f(n) = \Omega(g(n))$.

---

## 1.2 The Master Theorem for Divide-and-Conquer
For recurrences of the form:
$$T(n) = a \cdot T\left(\frac{n}{b}\right) + f(n), \quad \text{where } a \ge 1, \; b > 1$$

Compare $f(n)$ with $n^{\log_b a}$:

1. **Case 1 (Dominated by Subproblems)**:
   If $f(n) = O(n^{\log_b a - \epsilon})$ for some constant $\epsilon > 0$:
   $$T(n) = \Theta(n^{\log_b a})$$

2. **Case 2 (Equal Weights)**:
   If $f(n) = \Theta(n^{\log_b a} \log^k n)$ for some $k \ge 0$:
   $$T(n) = \Theta(n^{\log_b a} \log^{k+1} n)$$

3. **Case 3 (Dominated by Combine Cost)**:
   If $f(n) = \Omega(n^{\log_b a + \epsilon})$ for some constant $\epsilon > 0$, AND satisfies the **Regularity Condition** $a \cdot f(n/b) \le c \cdot f(n)$ for some $c < 1$:
   $$T(n) = \Theta(f(n))$$

---

# MODULE 2: DIVIDE AND CONQUER

## 2.1 Merge Sort
* **Concept**: Recursively divides array into two halves, sorts each half, and merges them in linear time.
* **Recurrence**:
  $$T(n) = 2 T(n/2) + \Theta(n)$$
* Applying Master Theorem ($a = 2, b = 2 \implies n^{\log_2 2} = n^1$):
  Case 2 applies ($k = 0$) $\implies \mathbf{T(n) = \Theta(n \log n)}$ for Best, Average, and Worst cases.
* **Auxiliary Space**: $\Theta(n)$ for temporary merge buffers.

---

## 2.2 Quick Sort
* **Concept**: Selects a pivot element and partitions the array such that all elements left of pivot are $\le$ pivot, and all right are $\ge$ pivot.
* **Partitioning Logic (Lomuto)**: Scans array with two pointers, swaps inverted elements, places pivot in final correct sorted position in $O(n)$ time.
* **Best Case ($T(n) = 2T(n/2) + O(n)$)**: Pivot splits array into equal halves $\implies \mathbf{O(n \log n)}$.
* **Worst Case ($T(n) = T(n-1) + O(n)$)**: Array already sorted/reverse sorted and first/last element chosen as pivot $\implies \mathbf{O(n^2)}$.
* **Randomized Quick Sort**: Choosing a random pivot guarantees expected runtime of $O(n \log n)$ with very high probability.

---

# MODULE 3: THE GREEDY METHOD

## 3.1 Characteristics of Greedy Algorithms
A problem exhibits greedy choice property if a locally optimal choice leads to a globally optimal solution. Requires **Optimal Substructure**.

## 3.2 Fractional Knapsack Problem
* Given $n$ items with values $v_i$ and weights $w_i$, and a knapsack capacity $W$.
* **Algorithm**:
  1. Calculate value-to-weight ratio $r_i = \frac{v_i}{w_i}$ for each item.
  2. Sort items in descending order of ratio $r_i$.
  3. Greedily take as much of the highest-ratio item as possible. If capacity remains, take a fraction $\frac{W_{\text{rem}}}{w_i}$ of the next item.
* **Time Complexity**: $O(n \log n)$ dominated by sorting.

---

## 3.3 Dijkstra's Shortest Path Algorithm
* Finds shortest path from single source vertex $s$ to all other vertices in a directed/undirected graph with **non-negative edge weights**.
* **Relaxation Step**: For edge $(u, v)$ with weight $w(u, v)$:
  $$\text{if } d[u] + w(u, v) < d[v] \implies d[v] = d[u] + w(u, v)$$
* **Time Complexity**: $O((V + E) \log V)$ using Min-Priority Queue.

---

# MODULE 4: DYNAMIC PROGRAMMING

## 4.1 Greedy vs. Dynamic Programming

| Attribute | Greedy Approach | Dynamic Programming |
| :--- | :--- | :--- |
| **Decision Making** | Makes locally best choice at each step; never backtracks | Evaluates all possibilities and memoizes subproblem solutions |
| **Subproblems** | Solves one subproblem | Solves overlapping subproblems and builds bottom-up |
| **Knapsack Problem** | Solves **Fractional Knapsack** optimally | Solves **0/1 Knapsack** optimally |
| **Optimality Guarantee** | May fail for non-greedy structures | Always guarantees globally optimal solution |

---

## 4.2 0/1 Knapsack Problem Formulation
* Given $n$ items with weights $w_1 \dots w_n$ and values $v_1 \dots v_n$, and maximum capacity $W$.
* Items cannot be divided; take item wholly ($x_i = 1$) or leave it ($x_i = 0$).

### Recurrence Relation:
Let $V[i, w]$ be the maximum value obtained using a subset of items $\{1 \dots i\}$ with weight limit $w$:
$$V[i, w] = \begin{cases} 
0 & \text{if } i = 0 \text{ or } w = 0 \\
V[i-1, w] & \text{if } w_i > w \\
\max\big(V[i-1, w], \; v_i + V[i-1, w - w_i]\big) & \text{if } w_i \le w 
\end{cases}$$
* **Time Complexity**: $O(n \cdot W)$ (Pseudo-polynomial).
* **Space Complexity**: $O(n \cdot W)$, optimizable to $O(W)$.

---

# MODULE 5: BACKTRACKING & NP-COMPLETENESS

## 5.1 Backtracking & The N-Queens Problem
* Place $N$ queens on an $N \times N$ chessboard such that no two queens attack each other (no two queens in same row, column, or diagonal).
* **Array Representation**: $X[k]$ represents the column position of the queen in row $k$.
* **Bounding Condition**: Queen $k$ at position $X[k]$ conflicts with queen $i$ at $X[i]$ if:
  $$X[i] == X[k] \quad \text{(same column)} \quad \lor \quad |X[i] - X[k]| == |i - k| \quad \text{(same diagonal)}$$

---

## 5.2 Complexity Classes: P, NP, NP-Complete, NP-Hard

```
      +-------------------------------------------+
      |                  NP-Hard                  |
      |   +-----------------------------------+   |
      |   |            NP-Complete            |   |
      |   |   (Hardest problems in NP)        |   |
      |   +-----------------+-----------------+   |
      |                     |                     |
      |       NP            |                     |
      |   +-------------+   |                     |
      |   |      P      |   |                     |
      |   | (Solvable in|   |                     |
      |   |  Poly-Time) |   |                     |
      |   +-------------+   |                     |
      +---------------------+---------------------+
```

1. **Class P (Polynomial Time)**: Decision problems that can be **solved** by a Deterministic Turing Machine in polynomial time $O(n^k)$ (e.g., Shortest Path, Minimum Spanning Tree, Sorting).
2. **Class NP (Nondeterministic Polynomial Time)**: Decision problems whose solutions can be **verified** in polynomial time by a deterministic algorithm (e.g., Travelling Salesperson decision version, Hamiltonian Cycle).
3. **Class NP-Hard**: A problem $X$ is NP-Hard if every problem in NP can be reduced to $X$ in polynomial time ($Y \le_P X, \forall Y \in \text{NP}$). $X$ does not need to be in NP itself.
4. **Class NP-Complete**: A problem $X$ is NP-Complete if:
   * $X \in \text{NP}$, AND
   * $X$ is **NP-Hard**.
5. **Cook's Theorem (1971)**: Proved that the **Boolean Satisfiability Problem (SAT)** is NP-Complete. All other NP-Complete proofs stem from reducing known NP-Complete problems to the target problem.

---

# 🎯 Model Exam Questions with Step-by-Step Solutions

### Q1. [Module 1 - 10 Marks]
**Solve the following recurrences using the Master Theorem:**
1. $T(n) = 4 T(n/2) + n^2$
2. $T(n) = 8 T(n/2) + n^2$
3. $T(n) = 2 T(n/2) + n \log n$

**Solution:**

#### Part 1: $T(n) = 4 T(n/2) + n^2$
* Here $a = 4, b = 2, f(n) = n^2$.
* Compute $n^{\log_b a} = n^{\log_2 4} = n^2$.
* Compare $f(n)$ with $n^{\log_b a}$: $f(n) = \Theta(n^2) = \Theta(n^{\log_b a} \log^0 n) \implies k = 0$.
* **Master Theorem Case 2 applies**:
  $$T(n) = \Theta(n^{\log_b a} \log^{k+1} n) = \mathbf{\Theta(n^2 \log n)}$$

#### Part 2: $T(n) = 8 T(n/2) + n^2$
* Here $a = 8, b = 2, f(n) = n^2$.
* Compute $n^{\log_b a} = n^{\log_2 8} = n^3$.
* Compare: $f(n) = n^2 = O(n^{3 - 1}) \implies \epsilon = 1 > 0$.
* **Master Theorem Case 1 applies**:
  $$T(n) = \mathbf{\Theta(n^3)}$$

#### Part 3: $T(n) = 2 T(n/2) + n \log n$
* Here $a = 2, b = 2, f(n) = n \log n$.
* Compute $n^{\log_b a} = n^{\log_2 2} = n^1$.
* Compare: $f(n) = \Theta(n^1 \log^1 n) \implies k = 1$.
* **Extended Master Theorem Case 2 applies**:
  $$T(n) = \Theta(n \log^{1+1} n) = \mathbf{\Theta(n \log^2 n)}$$

---

### Q2. [Module 4 - 10 Marks]
**Solve the 0/1 Knapsack Problem for $n = 4$ items and knapsack capacity $W = 5$ using Dynamic Programming:**

| Item ($i$) | Weight ($w_i$) | Value ($v_i$) |
| :---: | :---: | :---: |
| 1 | 2 | 12 |
| 2 | 1 | 10 |
| 3 | 3 | 20 |
| 4 | 2 | 15 |

**Construct the DP table and trace the selected subset of items.**

**Solution:**

#### 1. Construct the DP Table $V[i, w]$:
Formula: $V[i, w] = \max(V[i-1, w], \; v_i + V[i-1, w - w_i])$ if $w \ge w_i$, else $V[i-1, w]$.

| Item $i$ ($w_i, v_i$) | $w = 0$ | $w = 1$ | $w = 2$ | $w = 3$ | $w = 4$ | $w = 5$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **$i = 0$ (Initial)** | 0 | 0 | 0 | 0 | 0 | 0 |
| **$i = 1$ (2, 12)** | 0 | 0 | 12 | 12 | 12 | 12 |
| **$i = 2$ (1, 10)** | 0 | 10 | 12 | 22 | 22 | 22 |
| **$i = 3$ (3, 20)** | 0 | 10 | 12 | 22 | 30 | 32 |
| **$i = 4$ (2, 15)** | 0 | 10 | 15 | 25 | 30 | **37** |

* Maximum obtainable value = **37** (at $V[4, 5]$).

#### 2. Backtracking to Identify Selected Items:
* Compare $V[4, 5]$ ($37$) with $V[3, 5]$ ($32$):
  * $37 \neq 32 \implies$ **Item 4 is SELECTED!**
  * Remaining capacity $W = 5 - w_4 = 5 - 2 = 3$.
* Compare $V[3, 3]$ ($22$) with $V[2, 3]$ ($22$):
  * $22 == 22 \implies$ **Item 3 is NOT selected**.
  * Remaining capacity $W = 3$.
* Compare $V[2, 3]$ ($22$) with $V[1, 3]$ ($12$):
  * $22 \neq 12 \implies$ **Item 2 is SELECTED!**
  * Remaining capacity $W = 3 - w_2 = 3 - 1 = 2$.
* Compare $V[1, 2]$ ($12$) with $V[0, 2]$ ($0$):
  * $12 \neq 0 \implies$ **Item 1 is SELECTED!**
  * Remaining capacity $W = 2 - w_1 = 2 - 2 = 0$.

#### Final Answer:
* **Selected Items**: $\{\text{Item 1}, \text{Item 2}, \text{Item 4}\}$
* **Total Weight**: $w_1 + w_2 + w_4 = 2 + 1 + 2 = \mathbf{5} \le 5$.
* **Total Maximum Value**: $v_1 + v_2 + v_4 = 12 + 10 + 15 = \mathbf{37}$.
