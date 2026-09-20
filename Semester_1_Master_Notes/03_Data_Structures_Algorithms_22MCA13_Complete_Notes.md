# Master Study Notes: Data Structures with Algorithms
## Course Code: 22MCA13 / MMC203 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Linear Data Structures, Stacks ADT, Applications (Infix to Postfix, Postfix Evaluation, Tower of Hanoi), Queues ADT, Circular Queue, Deque, Priority Queue.
* **Module 2**: Dynamic Memory & Linked Lists, Singly Linked List, Doubly Linked List, Circular Linked List, Polynomial Representation & Addition.
* **Module 3**: Non-Linear Data Structures, Binary Trees, Binary Tree Properties & Traversals (Inorder, Preorder, Postorder, Level-Order), Binary Search Trees (BST) Insertion, Searching & Deletion.
* **Module 4**: Balanced Search Trees, AVL Trees, Balance Factors, 4 Rotations (LL, RR, LR, RL), B-Trees, Hashing Techniques, Hash Functions & Collision Resolution Strategies (Chaining vs Open Addressing).
* **Module 5**: Graphs & Traversals (BFS, DFS), Sorting Algorithms (Merge Sort, Quick Sort, Heap Sort), Stability, Time & Space Complexity Analysis.

---

# MODULE 1: STACKS & QUEUES

## 1.1 The Stack ADT
A **Stack** is a linear data structure operating on the **LIFO (Last-In, First-Out)** principle.
* **Operations**:
  * `push(x)`: Inserts element $x$ at the top. Condition: Check for **Stack Overflow** ($top == MAX - 1$).
  * `pop()`: Removes and returns top element. Condition: Check for **Stack Underflow** ($top == -1$).
  * `peek()`: Returns top element without removal.
  * Time Complexity: $O(1)$ for all basic operations.

### Applications of Stacks:
1. **Infix to Postfix Conversion**: Using operator precedence and associativity.
2. **Expression Evaluation**: Postfix and Prefix evaluation.
3. **Function Call Stack**: Managing recursion, local variables, and return addresses.
4. **Parentheses Matching**: Verifying balanced syntax in compilers.

---

## 1.2 The Queue ADT & Circular Queue
A **Queue** is a linear data structure operating on the **FIFO (First-In, First-Out)** principle.
* **Linear Queue Limitation**: False overflow occurs when $rear == MAX - 1$ even if spaces are vacated at the front.
* **Circular Queue**: Solves false overflow by wrapping around using modulo arithmetic.

### Circular Queue Mechanics:
* **Empty Condition**: $front == -1$ and $rear == -1$.
* **Full Condition**:
  $$(rear + 1) \pmod{MAX} == front$$
* **Enqueue Operation**:
  ```c
  if ((rear + 1) % MAX == front) {
      printf("Queue Overflow\n");
      return;
  }
  if (front == -1) front = 0;
  rear = (rear + 1) % MAX;
  queue[rear] = item;
  ```
* **Dequeue Operation**:
  ```c
  if (front == -1) {
      printf("Queue Underflow\n");
      return -1;
  }
  item = queue[front];
  if (front == rear) { // Reset queue when last item is dequeued
      front = -1;
      rear = -1;
  } else {
      front = (front + 1) % MAX;
  }
  return item;
  ```

---

# MODULE 2: LINKED LISTS

## 2.1 Singly Linked List (SLL)
A sequence of nodes where each node contains `data` and a `next` pointer.
```c
struct Node {
    int data;
    struct Node* next;
};
```
* **Insertion at Beginning**: $O(1)$. New node's `next` points to current `head`; `head` updated.
* **Insertion at End**: $O(n)$ without tail pointer. Traverse to last node whose `next == NULL`.
* **Reversal of Singly Linked List** ($O(n)$ time, $O(1)$ space):
  ```c
  struct Node* reverseList(struct Node* head) {
      struct Node *prev = NULL, *current = head, *next = NULL;
      while (current != NULL) {
          next = current->next;
          current->next = prev;
          prev = current;
          current = next;
      }
      return prev; // New head
  }
  ```

## 2.2 Doubly Linked List (DLL)
Each node contains `data`, `prev` pointer, and `next` pointer.
* Permits bidirectional traversal. Deletion of a known node is $O(1)$ without needing to traverse from head to find previous node.

---

# MODULE 3: BINARY TREES & BINARY SEARCH TREES

## 3.1 Binary Tree Properties
* A tree where each node has at most two children (`left`, `right`).
* Maximum nodes at level $i$: $2^i$ (root is level 0).
* Maximum nodes in binary tree of height $h$: $2^{h+1} - 1$.
* In any non-empty binary tree with $n_0$ leaf nodes and $n_2$ nodes of degree 2:
  $$n_0 = n_2 + 1$$

## 3.2 Tree Traversals
1. **Preorder (Root, Left, Right)**: Visit root $\to$ traverse left subtree $\to$ traverse right subtree.
2. **Inorder (Left, Root, Right)**: Produces sorted order for a Binary Search Tree.
3. **Postorder (Left, Right, Root)**: Used for expression trees and bottom-up resource freeing.
4. **Level-Order (Breadth-First)**: Implemented using a FIFO Queue.

---

## 3.3 Binary Search Tree (BST)
Property: For any node $X$, all keys in the left subtree are $< \text{key}(X)$, and all keys in the right subtree are $> \text{key}(X)$.

### Deletion in BST (3 Cases):
1. **Case 1: Node is a Leaf (0 children)**: Simply set the parent's pointer to `NULL` and free the node.
2. **Case 2: Node has 1 Child**: Bypass the node by linking the parent directly to the child node.
3. **Case 3: Node has 2 Children**:
   * Find the node's **Inorder Successor** (smallest element in right subtree) OR **Inorder Predecessor** (largest element in left subtree).
   * Copy the successor's data into the target node.
   * Recursively delete the inorder successor (which falls into Case 1 or Case 2).

---

# MODULE 4: AVL TREES & HASHING

## 4.1 AVL Trees (Adelson-Velsky and Landis)
A self-balancing Binary Search Tree where the **Balance Factor ($BF$)** of every node is strictly $-1, 0, \text{ or } +1$:
$$BF(\text{node}) = \text{Height}(\text{Left Subtree}) - \text{Height}(\text{Right Subtree})$$

### Four Rebalancing Rotations:
1. **LL Rotation (Single Right Rotation)**: Triggered when an insertion is made into the Left subtree of the Left child ($BF = +2$).
2. **RR Rotation (Single Left Rotation)**: Triggered when an insertion is made into the Right subtree of the Right child ($BF = -2$).
3. **LR Rotation (Double Rotation: Left then Right)**: Inserted into Right subtree of Left child ($BF = +2$). First perform left rotation on left child, then right rotation on root.
4. **RL Rotation (Double Rotation: Right then Left)**: Inserted into Left subtree of Right child ($BF = -2$). First perform right rotation on right child, then left rotation on root.

---

## 4.2 Hashing & Collision Resolution
* **Hash Function**: $h(k)$ maps key $k$ to table index $[0 \dots m-1]$.
  * Division Method: $h(k) = k \pmod m$ (where $m$ is a prime number).

### Collision Resolution Techniques:
1. **Separate Chaining (Open Hashing)**: Each table slot contains a linked list of records with the same hash key.
2. **Open Addressing (Closed Hashing)**: All records are stored directly in the hash table array:
   * **Linear Probing**: $h(k, i) = (h'(k) + i) \pmod m$. Suffers from **Primary Clustering**.
   * **Quadratic Probing**: $h(k, i) = (h'(k) + c_1 i + c_2 i^2) \pmod m$. Eliminates primary clustering.
   * **Double Hashing**: $h(k, i) = (h_1(k) + i \cdot h_2(k)) \pmod m$. Best open addressing performance.

---

# MODULE 5: GRAPHS & SORTING ALGORITHMS

## 5.1 Graph Traversals
* **Breadth-First Search (BFS)**: Uses a **Queue**. Explores neighbor nodes level-by-level. Finds the shortest path in unweighted graphs. Time: $O(V + E)$.
* **Depth-First Search (DFS)**: Uses a **Stack / Recursion**. Explores as deep as possible along each branch before backtracking. Used for cycle detection and topological sorting. Time: $O(V + E)$.

---

## 5.2 Sorting Algorithms Summary Table

| Algorithm | Best Time | Average Time | Worst Time | Space | Stable? | Key Mechanism |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **Bubble Sort** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes | Repeatedly swap adjacent inverted pairs. |
| **Insertion Sort** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes | Insert next element into sorted left sub-array. |
| **Selection Sort** | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | No | Select minimum and place at beginning. |
| **Merge Sort** | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | Yes | Divide array into halves, recursively sort, merge. |
| **Quick Sort** | $O(n \log n)$ | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ | No | Partition around a pivot element. |
| **Heap Sort** | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ | No | Build Max-Heap, swap root with end, max-heapify. |

---

# 🎯 Model Exam Questions with Step-by-Step Solutions

### Q1. [Module 1 - 10 Marks]
**Convert the following Infix expression to Postfix using the Stack algorithm and show the tabular trace:**
$$\mathbf{(A + B) * (C - D) / (E + F \uparrow G)}$$
*(Note: $\uparrow$ denotes exponentiation with highest precedence and right-to-left associativity).*

**Solution:**

| Symbol | Action / Rule | Stack (Bottom to Top) | Postfix Output Expression |
| :---: | :--- | :--- | :--- |
| `(` | Push to Stack | `(` | |
| `A` | Append to Output | `(` | `A` |
| `+` | Push to Stack | `( +` | `A` |
| `B` | Append to Output | `( +` | `A B` |
| `)` | Pop until `(` | *empty* | `A B +` |
| `*` | Push to Stack | `*` | `A B +` |
| `(` | Push to Stack | `* (` | `A B +` |
| `C` | Append to Output | `* (` | `A B + C` |
| `-` | Push to Stack | `* ( -` | `A B + C` |
| `D` | Append to Output | `* ( -` | `A B + C D` |
| `)` | Pop until `(` | `*` | `A B + C D -` |
| `/` | Precedence equal to `*`, pop `*`, push `/` | `/` | `A B + C D - *` |
| `(` | Push to Stack | `/ (` | `A B + C D - *` |
| `E` | Append to Output | `/ (` | `A B + C D - * E` |
| `+` | Push to Stack | `/ ( +` | `A B + C D - * E` |
| `F` | Append to Output | `/ ( +` | `A B + C D - * E F` |
| `^` | Higher precedence than `+`, push | `/ ( + ^` | `A B + C D - * E F` |
| `G` | Append to Output | `/ ( + ^` | `A B + C D - * E F G` |
| `)` | Pop until `(` (`^`, `+`) | `/` | `A B + C D - * E F G ^ +` |
| *End* | Pop remaining operators (`/`) | *empty* | `A B + C D - * E F G ^ + /` |

**Final Postfix Expression**:
$$\mathbf{A B + C D - * E F G \uparrow + /}$$

---

### Q2. [Module 3 - 10 Marks]
**Construct a Binary Search Tree (BST) by inserting the following sequence of keys into an initially empty tree:**
$$50, 30, 70, 20, 40, 60, 80, 10, 25, 65$$
**Then show the tree after deleting node $50$.**

**Solution:**
1. **Step-by-Step Insertion**:
   * Insert $50$: Root is `50`.
   * Insert $30$: $30 < 50 \implies$ Left child of `50`.
   * Insert $70$: $70 > 50 \implies$ Right child of `50`.
   * Insert $20$: $20 < 30 \implies$ Left child of `30`.
   * Insert $40$: $40 > 30 \implies$ Right child of `30`.
   * Insert $60$: $60 < 70 \implies$ Left child of `70`.
   * Insert $80$: $80 > 70 \implies$ Right child of `70`.
   * Insert $10$: $10 < 20 \implies$ Left child of `20`.
   * Insert $25$: $25 > 20 \implies$ Right child of `20`.
   * Insert $65$: $65 > 60 \implies$ Right child of `60`.

```
         50
       /    \
     30      70
    /  \    /  \
   20  40  60  80
  /  \       \
 10  25      65
```

2. **Inorder Traversal Check**:
   $10, 20, 25, 30, 40, 50, 60, 65, 70, 80$ (Perfect ascending order).

3. **Deleting Node $50$ (Node with 2 Children)**:
   * Find **Inorder Successor** of $50$: Smallest node in right subtree = `60`.
   * Replace $50$ with $60$.
   * Delete original node $60$ (which has only 1 child `65`). Node `70` adopts `65` as left child.

```
         60
       /    \
     30      70
    /  \    /  \
   20  40  65  80
  /  \
 10  25
```
