# VTU MCA 2022/2024 Scheme - Mathematical Foundations (22MCA11)
## Full 100-Mark Solved Examination Paper with Step-by-Step Numerical Solutions
Time: 3 Hours | Max Marks: 100

================================================================================
MODULE 1: SET THEORY & DISCRETE MATHEMATICS
================================================================================

Q.1 (a) State and prove the Principle of Inclusion-Exclusion for three finite sets A, B, and C. [10 Marks]
Answer:
Formula:
|A U B U C| = |A| + |B| + |C| - |A n B| - |B n C| - |A n C| + |A n B n C|

Step-by-step Proof:
1. Let D = B U C. Then A U B U C = A U D.
2. By two-set inclusion-exclusion: |A U D| = |A| + |D| - |A n D|.
3. Substitute D = B U C:
   |A U (B U C)| = |A| + (|B| + |C| - |B n C|) - |A n (B U C)|
4. By distributive law: A n (B U C) = (A n B) U (A n C).
5. Apply two-set rule to |(A n B) U (A n C)|:
   |(A n B) U (A n C)| = |A n B| + |A n C| - |(A n B) n (A n C)|
                       = |A n B| + |A n C| - |A n B n C|
6. Substitute back into step 3:
   |A U B U C| = |A| + |B| + |C| - |B n C| - (|A n B| + |A n C| - |A n B n C|)
               = |A| + |B| + |C| - |A n B| - |B n C| - |A n C| + |A n B n C|. (Proved)

Numerical Example:
In an MCA class of 100 students: 60 know Python, 50 know Java, 40 know C++. 30 know Python & Java, 20 know Java & C++, 25 know Python & C++, and 15 know all three.
Total knowing at least one language:
|P U J U C| = 60 + 50 + 40 - 30 - 20 - 25 + 15 = 90 students.
Students knowing none of the three = 100 - 90 = 10 students.

--------------------------------------------------------------------------------
Q.1 (b) What is an Equivalence Relation? Verify if relation R on Integers defined by a R b <=> (a - b) is divisible by 5 is an equivalence relation. [10 Marks]
Answer:
A relation R on set S is an Equivalence Relation if and only if it satisfies:
1. Reflexive: a R a for all a in S.
   Here: a - a = 0 = 5 * 0, which is divisible by 5. Hence Reflexive.
2. Symmetric: If a R b, then b R a.
   If (a - b) = 5k (k in Z), then (b - a) = -(a - b) = 5(-k).
   Since -k is an integer, (b - a) is divisible by 5. Hence Symmetric.
3. Transitive: If a R b and b R c, then a R c.
   If (a - b) = 5k and (b - c) = 5m, then:
   (a - c) = (a - b) + (b - c) = 5k + 5m = 5(k + m).
   Since (k + m) is an integer, (a - c) is divisible by 5. Hence Transitive.
Conclusion: Since R is Reflexive, Symmetric, and Transitive, R is an Equivalence Relation.

================================================================================
MODULE 2: MATRIX ALGEBRA & LINEAR EQUATIONS
================================================================================

Q.3 (a) Find the Eigenvalues and Eigenvectors of the Matrix A = [[4, 2], [1, 3]]. [10 Marks]
Answer:
Step 1: Characteristic equation det(A - lambda * I) = 0:
| 4 - lambda      2        |
|     1       3 - lambda   | = 0
(4 - lambda)(3 - lambda) - (2)(1) = 0
12 - 7*lambda + lambda^2 - 2 = 0
lambda^2 - 7*lambda + 10 = 0
(lambda - 5)(lambda - 2) = 0
Eigenvalues: lambda_1 = 5, lambda_2 = 2.

Step 2: Eigenvector for lambda_1 = 5:
(A - 5I)X = 0 => [[-1, 2], [1, -2]] [[x1], [x2]] = [[0], [0]]
-x1 + 2*x2 = 0 => x1 = 2*x2.
Let x2 = 1 => X_1 = [[2], [1]].

Step 3: Eigenvector for lambda_2 = 2:
(A - 2I)X = 0 => [[2, 2], [1, 1]] [[x1], [x2]] = [[0], [0]]
2*x1 + 2*x2 = 0 => x1 = -x2.
Let x2 = 1 => X_2 = [[-1], [1]].

Final Answer:
Eigenvalues: 5, 2.
Corresponding Eigenvectors: [2, 1]^T and [-1, 1]^T.

--------------------------------------------------------------------------------
Q.3 (b) Solve by Gauss-Elimination Method:
2x + y + z = 10
3x + 2y + 3z = 18
x + 4y + 9z = 16  [10 Marks]
Answer:
Augmented Matrix [A|B]:
[ 2  1  1 | 10 ]
[ 3  2  3 | 18 ]
[ 1  4  9 | 16 ]

Row Operations:
1. Swap R1 and R3:
[ 1  4  9 | 16 ]
[ 3  2  3 | 18 ]
[ 2  1  1 | 10 ]

2. R2 -> R2 - 3*R1:
   [ 3 - 3, 2 - 12, 3 - 27 | 18 - 48 ] = [ 0, -10, -24 | -30 ]
   Divide R2 by -2: [ 0, 5, 12 | 15 ]

3. R3 -> R3 - 2*R1:
   [ 2 - 2, 1 - 8, 1 - 18 | 10 - 32 ] = [ 0, -7, -17 | -22 ]

4. Eliminate y from R3 (R3 -> 5*R3 + 7*R2):
   5*(-17) + 7*(12) = -85 + 84 = -1 * z
   5*(-22) + 7*(15) = -110 + 105 = -5
   -z = -5 => z = 5.

5. Back Substitution:
   5y + 12(5) = 15 => 5y + 60 = 15 => 5y = -45 => y = -9.
   x + 4(-9) + 9(5) = 16 => x - 36 + 45 = 16 => x + 9 = 16 => x = 7.

Final Solution: x = 7, y = -9, z = 5.
Verification: 2(7) + (-9) + 5 = 14 - 9 + 5 = 10 (Correct).

================================================================================
MODULE 3: GRAPH THEORY & TREES
================================================================================

Q.5 (a) State and prove the Handshaking Lemma in Graph Theory. [10 Marks]
Answer:
Statement: In any undirected graph G = (V, E), the sum of degrees of all vertices equals twice the number of edges:
sum(deg(v)) = 2 * |E| for all v in V.

Proof:
1. Every edge e = (u, v) connects two vertices u and v.
2. When calculating deg(u), edge e is counted once.
3. When calculating deg(v), edge e is counted once again.
4. Hence, each edge contributes exactly 2 to the sum of degrees of vertices in G.
5. Therefore, sum_{v in V} deg(v) = 2 * |E|. (Proved)

Corollary: In any undirected graph, the number of vertices with odd degree is always EVEN.
Proof: Total sum is 2*|E| (even).
sum(deg(v)) = sum_{v is even}(deg(v)) + sum_{v is odd}(deg(v)) = Even.
Since sum of even degrees is even, sum of odd degrees must also be even.
Sum of odd integers can only be even if the count of terms is EVEN.

--------------------------------------------------------------------------------
Q.5 (b) Explain Prim's Algorithm for Minimum Spanning Tree (MST) with an example. [10 Marks]
Answer:
Prim's Algorithm is a greedy graph algorithm that starts with an arbitrary node and grows the MST by iteratively picking the minimum-weight edge connecting a visited node to an unvisited node.

Given Graph:
Vertices: {A, B, C, D}
Edges: (A, B)=1, (B, C)=4, (A, C)=3, (B, D)=2, (C, D)=5.

Execution Trace:
1. Start at Vertex A. Visited = {A}.
2. Available edges: (A, B)=1, (A, C)=3. Pick minimum: (A, B) weight 1.
   Visited = {A, B}, MST Edges = {(A, B)}.
3. Available edges: (A, C)=3, (B, C)=4, (B, D)=2. Pick minimum: (B, D) weight 2.
   Visited = {A, B, D}, MST Edges = {(A, B), (B, D)}.
4. Available edges: (A, C)=3, (B, C)=4, (C, D)=5. Pick minimum: (A, C) weight 3.
   Visited = {A, B, C, D}, MST Edges = {(A, B), (B, D), (A, C)}.
5. All vertices visited. Total MST Cost = 1 + 2 + 3 = 6.

================================================================================
MODULE 4: PROBABILITY & RANDOM VARIABLES
================================================================================

Q.7 (a) State Bayes' Theorem and solve: In a factory, Machine A produces 60% of items (2% defective) and Machine B produces 40% of items (4% defective). An item picked at random is defective. What is the probability it came from Machine B? [10 Marks]
Answer:
Bayes' Theorem:
P(B|D) = [ P(B) * P(D|B) ] / [ P(A)*P(D|A) + P(B)*P(D|B) ]

Given:
P(A) = 0.60, P(D|A) = 0.02
P(B) = 0.40, P(D|B) = 0.04

Total Probability of Defective Item P(D):
P(D) = (0.60 * 0.02) + (0.40 * 0.04)
     = 0.012 + 0.016 = 0.028 (2.8%)

Probability that defective item was produced by Machine B:
P(B|D) = (0.40 * 0.04) / 0.028 = 0.016 / 0.028 = 16 / 28 = 4/7 = 0.5714 (57.14%).

================================================================================
MODULE 5: STATISTICAL INFERENCE & HYPOTHESIS TESTING
================================================================================

Q.9 (a) Explain Null Hypothesis, Type I and Type II errors with real-world examples. [10 Marks]
Answer:
1. Null Hypothesis (H0): Statement of status-quo or no significant effect. Example: "A new machine does not increase production speed."
2. Alternative Hypothesis (H1): Statement that contradicts H0.
3. Type I Error (Alpha Error): Rejecting H0 when H0 is actually TRUE (False Positive).
   Example: Convicting an innocent person in court.
4. Type II Error (Beta Error): Failing to reject H0 when H0 is actually FALSE (False Negative).
   Example: Letting a guilty criminal walk free.
5. Level of Significance (alpha): Usually set to 0.05 (5%) or 0.01 (1%).
