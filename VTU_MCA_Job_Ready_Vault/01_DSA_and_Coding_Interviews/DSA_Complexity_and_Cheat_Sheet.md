# Data Structures & Big-O Complexity Cheat Sheet

---

## 1. Core Data Structures Time & Space Complexity

| Data Structure | Access (Avg) | Search (Avg) | Insertion (Avg) | Deletion (Avg) | Access (Worst) | Search (Worst) | Insertion (Worst) | Deletion (Worst) | Space Complexity |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Array** | $O(1)$ | $O(N)$ | $O(N)$ | $O(N)$ | $O(1)$ | $O(N)$ | $O(N)$ | $O(N)$ | $O(N)$ |
| **Dynamic Array (List/ArrayList)** | $O(1)$ | $O(N)$ | $O(1)$ amortized | $O(N)$ | $O(1)$ | $O(N)$ | $O(N)$ | $O(N)$ | $O(N)$ |
| **Singly Linked List** | $O(N)$ | $O(N)$ | $O(1)$ (at head) | $O(1)$ (known node)| $O(N)$ | $O(N)$ | $O(1)$ | $O(1)$ | $O(N)$ |
| **Doubly Linked List** | $O(N)$ | $O(N)$ | $O(1)$ | $O(1)$ | $O(N)$ | $O(N)$ | $O(1)$ | $O(1)$ | $O(N)$ |
| **Stack (LIFO)** | $O(N)$ | $O(N)$ | $O(1)$ (push) | $O(1)$ (pop) | $O(N)$ | $O(N)$ | $O(1)$ | $O(1)$ | $O(N)$ |
| **Queue (FIFO)** | $O(N)$ | $O(N)$ | $O(1)$ (enqueue) | $O(1)$ (dequeue) | $O(N)$ | $O(N)$ | $O(1)$ | $O(1)$ | $O(N)$ |
| **Hash Table / Map** | N/A | $O(1)$ | $O(1)$ | $O(1)$ | N/A | $O(N)$ | $O(N)$ | $O(N)$ | $O(N)$ |
| **Binary Search Tree (BST)** | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | $O(N)$ | $O(N)$ | $O(N)$ | $O(N)$ | $O(N)$ |
| **Balanced BST (AVL / Red-Black)** | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ | $O(N)$ |
| **Binary Heap (Min/Max Heap)** | N/A | $O(N)$ | $O(\log N)$ | $O(\log N)$ (pop min/max) | N/A | $O(N)$ | $O(\log N)$ | $O(\log N)$ | $O(N)$ |
| **Trie (Prefix Tree)** | N/A | $O(L)$ | $O(L)$ | $O(L)$ | N/A | $O(L)$ | $O(L)$ | $O(L)$ | $O(\Sigma \cdot L \cdot N)$ |

*Note: $L$ = length of key string, $\Sigma$ = alphabet size (e.g. 26).*

---

## 2. Sorting Algorithms Comparison

| Algorithm | Best Time | Average Time | Worst Time | Worst Space | Stable? | In-Place? | Key Use Case |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Quicksort** | $O(N \log N)$ | $O(N \log N)$ | $O(N^2)$ (bad pivot)| $O(\log N)$ | No | Yes | General in-memory sorting |
| **Mergesort** | $O(N \log N)$ | $O(N \log N)$ | $O(N \log N)$ | $O(N)$ | **Yes** | No | Linked lists, external sorting |
| **Heapsort** | $O(N \log N)$ | $O(N \log N)$ | $O(N \log N)$ | $O(1)$ | No | **Yes** | Systems with strict memory limits |
| **Timsort (Python / Java sort)** | $O(N)$ | $O(N \log N)$ | $O(N \log N)$ | $O(N)$ | **Yes** | No | Real-world hybrid (Merge + Insertion) |
| **Insertion Sort** | $O(N)$ | $O(N^2)$ | $O(N^2)$ | $O(1)$ | **Yes** | **Yes** | Small datasets ($N \le 30$) or nearly sorted |
| **Counting Sort** | $O(N + K)$ | $O(N + K)$ | $O(N + K)$ | $O(K)$ | **Yes** | No | Small integer keys ($K \le 10^6$) |
