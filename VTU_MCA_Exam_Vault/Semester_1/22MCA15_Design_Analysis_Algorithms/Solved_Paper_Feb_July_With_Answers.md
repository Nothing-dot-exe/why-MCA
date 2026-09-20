# VTU MCA 2022/2024 Scheme - Design & Analysis of Algorithms (22MCA15)
## Full Solved Examination Paper with Proofs & Recurrence Relations
Time: 3 Hours | Max Marks: 100

================================================================================
MODULE 1: ASYMPTOTIC NOTATION & DIVIDE-AND-CONQUER
================================================================================

Q.1 (a) Define Big-O, Big-Omega, and Big-Theta asymptotic notations with mathematical definitions and diagrams. [10 Marks]
Answer:
1. Big-O (O) - Asymptotic Upper Bound (Worst-Case):
   f(n) = O(g(n)) iff there exist positive constants c and n0 such that:
   0 <= f(n) <= c * g(n) for all n >= n0.
   Significance: Guarantees that algorithm runtime will not exceed c * g(n).

2. Big-Omega (Omega) - Asymptotic Lower Bound (Best-Case):
   f(n) = Omega(g(n)) iff there exist positive constants c and n0 such that:
   0 <= c * g(n) <= f(n) for all n >= n0.
   Significance: Guarantees algorithm takes at least this much time.

3. Big-Theta (Theta) - Asymptotically Tight Bound (Average-Case):
   f(n) = Theta(g(n)) iff there exist positive constants c1, c2 and n0 such that:
   0 <= c1 * g(n) <= f(n) <= c2 * g(n) for all n >= n0.

--------------------------------------------------------------------------------
Q.1 (b) Solve the recurrence relation for Merge Sort using Master Theorem:
T(n) = 2*T(n/2) + Theta(n). [10 Marks]
Answer:
Master Theorem Form: T(n) = a*T(n/b) + f(n)
Here: a = 2, b = 2, f(n) = Theta(n) = Theta(n^c) where c = 1.
Calculate log_b(a) = log_2(2) = 1.
Compare c and log_b(a):
c = 1 and log_b(a) = 1 => c = log_b(a).
By Case 2 of Master Theorem:
T(n) = Theta(n^(log_b(a)) * log(n)) = Theta(n * log(n)).
Hence, Merge Sort has a time complexity of O(n log n) in all cases (Best, Worst, Average).

================================================================================
MODULE 2: DYNAMIC PROGRAMMING - 0/1 KNAPSACK
================================================================================

Q.3 (a) Solve the 0/1 Knapsack problem using Dynamic Programming for capacity W = 5:
Item | Weight | Value
1    | 2      | $12
2    | 1      | $10
3    | 3      | $20
4    | 2      | $15  [10 Marks]
Answer:
DP Table DP[i][w] = Max profit considering items 1..i and capacity w:
Formula:
If weight[i] > w: DP[i][w] = DP[i-1][w]
Else: DP[i][w] = max(DP[i-1][w], value[i] + DP[i-1][w - weight[i]])

DP Table:
Item (i) \ w |  0  |  1   |  2   |  3   |  4   |  5
--------------------------------------------------
0 (No item) |  0  |  0   |  0   |  0   |  0   |  0
1 (w=2, v=12)| 0  |  0   |  12  |  12  |  12  |  12
2 (w=1, v=10)| 0  |  10  |  12  |  22  |  22  |  22
3 (w=3, v=20)| 0  |  10  |  12  |  22  |  30  |  32
4 (w=2, v=15)| 0  |  10  |  15  |  25  |  30  |  37

Maximum Profit: DP[4][5] = $37.
Items selected by backtracking:
- Item 4 (w=2, v=15) included => remaining capacity = 5 - 2 = 3.
- Item 2 (w=1, v=10) included => remaining capacity = 3 - 1 = 2.
- Item 1 (w=2, v=12) included => remaining capacity = 2 - 2 = 0.
Total Value = 15 + 10 + 12 = $37 (Weight = 2 + 1 + 2 = 5 <= W).
