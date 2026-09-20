# VTU MCA 2022/2024 Scheme - Data Structures with Algorithms (22MCA13)
## Full Solved Examination Paper with Code & Step-by-Step Traces
Time: 3 Hours | Max Marks: 100

================================================================================
MODULE 1: LINEAR DATA STRUCTURES - STACKS & QUEUES
================================================================================

Q.1 (a) Explain Infix to Postfix Conversion using a Stack. Convert the expression:
(A + B) * (C - D) / E ^ F. [10 Marks]
Answer:
Operator Precedence and Associativity:
- Parentheses () : Highest
- Exponentiation (^) : Right-to-Left
- Multiplication (*), Division (/) : Left-to-Right
- Addition (+), Subtraction (-) : Left-to-Right

Step-by-Step Stack Trace Table:
Token | Stack Content | Postfix Output Buffer
(     | (             |
A     | (             | A
+     | ( +           | A
B     | ( +           | A B
)     | [empty]       | A B +
*     | *             | A B +
(     | * (           | A B +
C     | * (           | A B + C
-     | * ( -         | A B + C
D     | * ( -         | A B + C D
)     | *             | A B + C D -
/     | /             | A B + C D - *
E     | /             | A B + C D - * E
^     | / ^           | A B + C D - * E
F     | / ^           | A B + C D - * E F
[End] | [empty]       | A B + C D - * E F ^ /

Final Postfix Expression: A B + C D - * E F ^ /

--------------------------------------------------------------------------------
Q.1 (b) Implement a Circular Queue using an array in C with enqueue and dequeue operations. [10 Marks]
Answer:
```c
#include <stdio.h>
#define SIZE 5

int queue[SIZE];
int front = -1, rear = -1;

int isFull() {
    return (front == (rear + 1) % SIZE);
}

int isEmpty() {
    return (front == -1);
}

void enqueue(int val) {
    if (isFull()) {
        printf("Queue Overflow! Cannot insert %d\n", val);
        return;
    }
    if (isEmpty()) {
        front = rear = 0;
    } else {
        rear = (rear + 1) % SIZE;
    }
    queue[rear] = val;
    printf("Inserted %d at position %d\n", val, rear);
}

int dequeue() {
    if (isEmpty()) {
        printf("Queue Underflow!\n");
        return -1;
    }
    int val = queue[front];
    if (front == rear) {
        front = rear = -1; // Reset to empty
    } else {
        front = (front + 1) % SIZE;
    }
    printf("Removed %d from queue\n", val);
    return val;
}
```

================================================================================
MODULE 2: NON-LINEAR DATA STRUCTURES - BINARY SEARCH TREES (BST)
================================================================================

Q.3 (a) Construct a Binary Search Tree (BST) from the keys: 50, 30, 70, 20, 40, 60, 80, 10, 25. Show Inorder, Preorder, and Postorder traversals. [10 Marks]
Answer:
Tree Construction:
- 50 is Root
- 30 < 50 (Left child of 50)
- 70 > 50 (Right child of 50)
- 20 < 30 (Left child of 30)
- 40 > 30 (Right child of 30)
- 60 < 70 (Left child of 70)
- 80 > 70 (Right child of 70)
- 10 < 20 (Left child of 20)
- 25 > 20 (Right child of 20)

Visual BST Structure:
             50
           /    \
         30      70
        /  \    /  \
       20   40 60   80
      /  \
     10   25

Traversals:
1. Inorder (Left, Root, Right - Always Sorted):
   10, 20, 25, 30, 40, 50, 60, 70, 80
2. Preorder (Root, Left, Right):
   50, 30, 20, 10, 25, 40, 70, 60, 80
3. Postorder (Left, Right, Root):
   10, 25, 20, 40, 30, 60, 80, 70, 50
