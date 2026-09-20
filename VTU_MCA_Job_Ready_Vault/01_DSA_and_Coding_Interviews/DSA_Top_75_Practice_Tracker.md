# Blind 75 / NeetCode 75 Practice Tracker
**Target**: Solve and master all 75 canonical interview problems before your job interviews.

---

## 📊 Category Breakdown

### 1. Arrays & Hashing
- [ ] **Two Sum** (Easy) — Hash Map lookup | Time: $O(N)$, Space: $O(N)$
- [ ] **Contains Duplicate** (Easy) — Hash Set lookup | Time: $O(N)$
- [ ] **Valid Anagram** (Easy) — Frequency array or counter | Time: $O(N)$
- [ ] **Group Anagrams** (Medium) — Sorted string or char tuple as map key | Time: $O(N \cdot K \log K)$
- [ ] **Top K Frequent Elements** (Medium) — Bucket Sort or Min-Heap | Time: $O(N)$
- [ ] **Product of Array Except Self** (Medium) — Prefix & Suffix products | Time: $O(N)$, Space: $O(1)$
- [ ] **Longest Consecutive Sequence** (Medium) — Set lookups with sequence start check | Time: $O(N)$

### 2. Two Pointers & Sliding Window
- [ ] **Valid Palindrome** (Easy) — Left & Right two pointers | Time: $O(N)$
- [ ] **3Sum** (Medium) — Sort + Two Pointers for each index | Time: $O(N^2)$
- [ ] **Container With Most Water** (Medium) — Greedy two pointers narrowing width | Time: $O(N)$
- [ ] **Best Time to Buy and Sell Stock** (Easy) — Single pass track min price | Time: $O(N)$
- [ ] **Longest Substring Without Repeating Characters** (Medium) — Dynamic sliding window | Time: $O(N)$
- [ ] **Longest Repeating Character Replacement** (Medium) — Sliding window with max freq tracking | Time: $O(N)$
- [ ] **Minimum Window Substring** (Hard) — Sliding window with frequency map matching | Time: $O(N)$

### 3. Stack
- [ ] **Valid Parentheses** (Easy) — LIFO stack matching pairs | Time: $O(N)$
- [ ] **Min Stack** (Medium) — Auxiliary stack storing running minimums | Time: $O(1)$ all ops
- [ ] **Daily Temperatures** (Medium) — Monotonic decreasing stack | Time: $O(N)$
- [ ] **Largest Rectangle in Histogram** (Hard) — Monotonic increasing stack calculating width spans | Time: $O(N)$

### 4. Binary Search
- [ ] **Binary Search** (Easy) — Iterative low/high pointers | Time: $O(\log N)$
- [ ] **Search a 2D Matrix** (Medium) — Map 1D index to 2D row/col | Time: $O(\log(M \cdot N))$
- [ ] **Find Minimum in Rotated Sorted Array** (Medium) — Compare mid with right boundary | Time: $O(\log N)$
- [ ] **Search in Rotated Sorted Array** (Medium) — Identify which half is sorted | Time: $O(\log N)$

### 5. Linked Lists
- [ ] **Reverse Linked List** (Easy) — 3 pointers: `prev`, `curr`, `next` | Time: $O(N)$
- [ ] **Merge Two Sorted Lists** (Easy) — Dummy head pointer comparison | Time: $O(N)$
- [ ] **Reorder List** (Medium) — Find mid, reverse second half, weave together | Time: $O(N)$
- [ ] **Remove Nth Node From End of List** (Medium) — Fast/Slow pointer with N-gap | Time: $O(N)$
- [ ] **Linked List Cycle** (Easy) — Floyd's tortoise & hare | Time: $O(N)$, Space: $O(1)$
- [ ] **Merge K Sorted Lists** (Hard) — Min-Heap storing head nodes | Time: $O(N \log K)$

### 6. Trees & Binary Search Trees
- [ ] **Invert Binary Tree** (Easy) — Postorder / Preorder recursive swap | Time: $O(N)$
- [ ] **Maximum Depth of Binary Tree** (Easy) — `1 + max(left, right)` | Time: $O(N)$
- [ ] **Same Tree** (Easy) — Structural and value equality check | Time: $O(N)$
- [ ] **Subtree of Another Tree** (Easy) — Check equality at every candidate node | Time: $O(M \cdot N)$
- [ ] **Lowest Common Ancestor of a BST** (Medium) — Exploit BST property ($O(H)$)
- [ ] **Binary Tree Level Order Traversal** (Medium) — Queue BFS | Time: $O(N)$
- [ ] **Validate Binary Search Tree** (Medium) — Bounds checking `(min_val, max_val)` | Time: $O(N)$
- [ ] **Kth Smallest Element in a BST** (Medium) — Inorder traversal yields sorted order | Time: $O(N)$
- [ ] **Binary Tree Maximum Path Sum** (Hard) — Postorder contribution calculation | Time: $O(N)$

### 7. Graphs
- [ ] **Number of Islands** (Medium) — Grid BFS/DFS with visited marking | Time: $O(M \cdot N)$
- [ ] **Clone Graph** (Medium) — DFS with node-to-clone hash map | Time: $O(V + E)$
- [ ] **Pacific Atlantic Water Flow** (Medium) — Reverse DFS from ocean borders | Time: $O(M \cdot N)$
- [ ] **Course Schedule (Cycle Detection)** (Medium) — Topological Sort (Kahn's / DFS in-degree) | Time: $O(V + E)$
- [ ] **Number of Connected Components** (Medium) — Disjoint Set Union (DSU) / BFS | Time: $O(V + E)$

### 8. Dynamic Programming
- [ ] **Climbing Stairs** (Easy) — Fibonacci DP: $dp[i] = dp[i-1] + dp[i-2]$ | Time: $O(N)$, Space: $O(1)$
- [ ] **House Robber** (Medium) — Include/Exclude DP | Time: $O(N)$
- [ ] **Coin Change** (Medium) — Unbounded knapsack style bottom-up DP | Time: $O(A \cdot C)$
- [ ] **Longest Increasing Subsequence** (Medium) — Patience sorting / DP + Binary Search | Time: $O(N \log N)$
- [ ] **Word Break** (Medium) — DP boolean check with dictionary set | Time: $O(N^2)$
- [ ] **Longest Common Subsequence** (Medium) — 2D DP matrix string matching | Time: $O(M \cdot N)$
- [ ] **Unique Paths** (Medium) — Combinatorics or 2D grid DP | Time: $O(M \cdot N)$
